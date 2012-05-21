---
layout: post
title: "Chef recipe to install a PostgreSQL server on a machine configured with en_US locales"
date: 2012-05-22 00:00
comments: true
categories: [chef, sysadmin, unix, vagrant, database]

---
If you have tried to install a PostgreSQL server on a Vagrant box and you're not an english-native, you may have faced an UTF8 issue:

```
CREATE DATABASE db_mydb OWNER my_user ENCODING 'UTF8' TEMPLATE template1;
createdb: database creation failed: ERROR:  encoding UTF8 does not match locale en_US
DETAIL:  The chosen LC_CTYPE setting requires encoding LATIN1
```

This is due to your Vagrant base box being configured with `en_US` locales. When the PostgreSQL server is installed, it is configured according to these locales, and gets an `en_US` config which is not compatible with `UTF8` encoding databases.

<!-- more -->

Manually, you would change your server's locales before running the PostgreSQL installation.

To do this using Chef, this will take you 2 additional steps.

### 1. Configure the machine's locales permanently

An approach which worked for me is to add a `/etc/profile.d/lang.sh` file which will configure your shell with the appropriate locale-related environment variables. Here is the template script:

```bash lang.sh
export LANGUAGE="en_US.UTF-8"
export LANG="en_US.UTF-8"
export LC_ALL="en_US.UTF-8"
```

Once this is done, you need to run some commands on the server to prepare the locale files:

```
locale-gen en_US.UTF-8
dpkg-reconfigure locales
```

I've build a small cookbook as part of my [vagrant-stacks](https://github.com/rchampourlier/vagrant-stacks) repo, you can find it [there](https://github.com/rchampourlier/vagrant-stacks/tree/master/cookbooks_local/set_locale).

### 2. Install the PostgreSQL server with the appropriate environment variables

After running the locale configuration from your Chef recipe, you may want to continue with the PostgreSQL install. However, even if we've added the `lang.sh` initialization script, it hadn't the chance to be loaded yet. So the required environment variables are not set yet.

To ensure these environment variables get loaded when we run our `postgresql::server` recipe, we will encapsulate this call in a small recipe:

```ruby
ENV['LANGUAGE'] = ENV['LANG'] = ENV['LC_ALL'] = "en_US.UTF-8"
include_recipe "postgresql::server"
```

You can find this mini-recipe [here](https://github.com/rchampourlier/vagrant-stacks/tree/master/cookbooks_local/postgresql_server_utf8) again in my vagrant-stacks repo!

**That should be all!**

*If I forgot some step or you can't manage to get this working, do not hesitate to write a comment. I'll do my best to answer!*