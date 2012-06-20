---
layout: post
title: "Adding SSL on some pages of your Rails application"
date: 2012-03-23 09:49
comments: true
categories: [development, sysadmin, rubyonrails, unix, nginx]

---

This tutorial should help you to setup SSL on your *staging* and *production* servers as well as update your Rails application to require SSL for specific controller actions.

<!-- more -->

## Generate a self-signed certificate to test on your staging server

We start by creating a self-signed certificate you will be able to use on your test/staging server, before buying a real certificate and configuring your production machine with it.

### The terminal commands

*On your server console*

	openssl genrsa -aes256 -out server.key 2048 # step 1
	openssl req -new -key server.key -out server.csr # step 2
	mv server.key server.key.bak # step 3
	openssl rsa -in server.key.bak -out server.key # step 4
	sudo openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt # step 5

### What does it do?

1. Generates a private/public key pair
2. Creates a certificate request
	- You **must** fill every requested field unless it is marked optional.
	- In France, you should use the name of the "departement" to fill the _State or Province Name_.
	- Use your company name for _Organization Name_.
	- Be sure to fill the chosen website domain name for _Common Name_. **Don't fill your personal name for a website SSL certificate request!**
3. Copies the passphrase-protected private key to `server.key.bak`.
4. Removes the passphrase from the private key. You have to do this, so that it won't be needed when starting the webserver. Since this is probably done automatically (at server boot or through automatic management tools), you won't be there to enter the passphrase. **Be sure however that your private key is secure (root user access only), and revoke it as soon as you think it was compromised.** _If you didn't remove it, `server.key.bak` contains your passphrase-protected key, while `server.key` is the passphrase-free one._
5. Generates the self-signed certificate.

### How to fill, a French example

* Country Name: FR
* State or Province Name: Paris
* Locality Name: Paris
* Organization Name: Avdice
* Organizational Unit Name: IT Service
* Common Name: www.website.com
* Email Address: mail@website.com

_Nothing for challenge password and optional company name._

## Generate the Certificate Signing Request to get a real SSL certificate for your production server

The process is exactly the same as the one above, except you do not do step 5. You will buy a certificate from a provider, send it the CSR file you've created thanks to the steps 1 and 2, and it will provide you with the certificate.

## Move the files to the correct place

	sudo mv server.crt /etc/ssl/application_name.crt
	sudo mv server.key /etc/ssl/private/application_name.key
	
## Change the permissions of the files to make them safe

	sudo chown root:ssl-cert /etc/ssl/private/application_name.key
	sudo chmod 0600 /etc/ssl/private/application_name.key
	sudo chmod 0600 /etc/ssl/certs/application_name.crt 
	sudo chown root:ssl-cert /etc/ssl/certs/application_name.crt 
	
## Update the configuration of your webserver to add a SSL host

*This tutorial is written for nginx v0.7.65. It's quite old but it's the default package on Ubuntu 10.04.3 LTS. I however recommend you to update it by installing it directly from the source, it's quite easy!*

Adding the SSL host to your nginx configuration file essentially means to add the following lines:

	server {
		listen <%= app_port_ssl %>;
		client_max_body_size 500M;
		server_name <%= server_name %>;
		ssl                     on;
		ssl_certificate         /etc/ssl/certs/application_name.crt;
		ssl_certificate_key     /etc/ssl/private/application_name.key;
		ssl_session_timeout     5m;
		
		[...remaining configuration]
	}
	
You have to make some changes for the location you're proxying to Rails too.

In our case, we have this root location:

	location / {
	  # Returns file if it exists, else send to app server's location
	  try_files $uri @app_server;
	}

Our standard (ie. HTTP) `@app_server` location is defined like this:

	location @app_server {
	  proxy_set_header X-Forwarded-For$proxy_add_x_forwarded_for;
	  proxy_set_header Host $http_host;
	  proxy_pass http://<%= application %>_app_server;
	}

The one for SSL is like this:

	location @app_server {
	  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	  proxy_set_header X-Forwarded-Proto https;
	  proxy_set_header Host $http_host;
	  proxy_pass http://<%= application %>_app_server;
	  proxy_redirect off;
	  proxy_max_temp_file_size 0;
	}
	
You see some additional proxy directives, in particular `proxy_set_header X-Forwarded-Proto https;`, they ensure that Rails can correctly detect the request was made through HTTPS, not HTTP.

The remaining configuration may be a copy of your HTTP server configuration, in which case your server will do both HTTP and HTTPS the same way, or a specific configuration if you want to process different locations, perform redirections, etc.

## Update Rails application to require SSL for specific actions

Our goal is to have only specific controller actions requiring SSL. We don't want everything to be secured, this would unnecessarily increase our server's load.

We assume that our webserver-level configuration (nginx in our case) is not managing HTTP/HTTPS routing (we could do this through webserver rules, however we consider that this is business/app-level related, so this should be done at app-level).

*This tutorial is made for Rails 3.0.x (tested with 3.0.12)*

### Install `ssl_requirement` gem

Use this fork: <https://github.com/bartt/ssl_requirement> and follow the guide.

**If you want the short way...**

Add to your Gemfile

	gem 'bartt-ssl_requirement', '~>1.4.0', :require => 'ssl_requirement'


### Configure Devise for SSL

Add the following code at the bottom of your `Application` class definition within `config/application.rb`:

	config.to_prepare do
	  Devise::RegistrationsController.ssl_required :new, :create
	  Devise::SessionsController.ssl_required :new, :create
	end

Add to your `ApplicationController`

	include ::SslRequirement
	
For any other controller action you want SSL, add this  to your controller class:

	ssl_required :action_name
	
### Review routes when SSL is required

You probably don't want to limit all your secured routes to SSL. In most cases, it's better if you don't, so that the user gets a redirection when he tries to access through `http` a route with ssl required.

However, you can't do so for `POST` routes: redirection won't work, since it will redirect to a `GET`. For these routes, you can add the SSL constraint on the route, so that the user get a `404` error when he tries to mess with the URL's protocol!

If you're using Devise, you'll probably want to do this for you `user_registration` and `user_session` `POST` routes.

You will have to override Devise's defaults routes for `sessions` and `registrations` controllers. For the override, you'll use:

	devise_for  :users, 
	  :skip => [:registrations, :sessions]
	
This simply tells Devise to skip the registrations and sessions routes, as you'll give them yourself to enforce the SSL requirement.

And now, you create these routes by yourself:

``` ruby
	devise_scope :user do
	  scope '/users' do
      
        # Setting up registrations routes manually for https on create
        get '/cancel',        :to => 'devise/registrations#cancel', :as => :cancel_user_registration
        post '',              :to => 'devise/registrations#create',
                              :constraints => { :protocol => secured_protocol },
                              :as => :user_registration
        get '/sign_up',       :to => 'devise/registrations#new', :as => :new_user_registration
        get '/edit',          :to => 'devise/registrations#edit', :as => :edit_user_registration
        put '',               :to => 'devise/registrations#update'
        delete '',            :to => 'devise/registrations#destroy'
      
        # Sessions routes for https on create
        get '/sign_in',       :to => 'devise/sessions#new', :as => :new_user_session
        post '/sign_in',      :to => 'devise/sessions#create', :as => :user_session,
                              :constraints => { :protocol => secured_protocol }
        delete '/sign_out',   :to => 'devise/sessions#destroy', :as => :destroy_user_session
      end
	end
```

## Change your links to go `https`

This involves using the `_url` helper instead of the `_path` one, and adding this option: `:protocol => "https"`.

Be sure to change the url of forms matching SSL-enabled routes, or you'll get `404`s!

## Testing

You should now be able to test this configuration with your browser. You will need to deploy your updated app to a staging machine, unless your development is SSL-capable.

### With a self-signed SSL certificate (on staging)

The browser **will** complain that the certificate is not valid. It's normal, since it is not signed by a valid certificate authority. **You are indeed the certificate authority** (remember, it's self-signed), and **you are not known by the browser as a valid certificate authority!**

### With a true SSL certificate (on production)

You may encounter some issues with your browser recognizing the issued certificate. The browser may complain it is signed by an unknown authority. This may happen when an unknown intermediate certification authority is used. In this case, you have to modify your certificate file to include the complete chain of certificates. Be sure to insert the certificates from the chain **above** your site's certificate!

An example line to do this:

```
cat bundled_certificate_chain.crt >> server.crt
```

You can use this service to check your server is sending the expected certificate chain: <http://www.sslshopper.com/ssl-checker.html>
	
## References

* <http://wiki.nginx.org/HttpSslModule>