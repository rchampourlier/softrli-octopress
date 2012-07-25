---
layout: post
title: "PostgreSQL schema's owner altered in database dump preventing Rails from finding the data"
date: 2012-07-25 15:40
comments: true
published: true
categories: [rails, database, postgresql]

---

This is more a note-to-myself post, but if someone falls in the same hole, this may be helpful as I couldn't found any help on this on the web...

I have a Rails application whose database is regularly backed up (thanks to the [Backup gem](https://github.com/meskyanichi/backup/)). While updating my development environment (which is under Vagrant) and trying a migration to Rails 3.2, I was bitten by a hard-to-debug **issue involving PostgreSQL schemas**.

After creating the db (with `rake db:create`) and before loading the dump (using `psql -d app_development -f app.sql`), everything was fine: Rails could find the tables, tell which schema it was working on (returning `public` when running `ActiveRecord::Base.connection.current_schema` in the console).

However, as soon as I loaded the dump file, Rails could not find the data anymore (more precisely the schema). It could still access the database itself, but not the schema. Related issues were:

* Rails keeping saying `...(Table doesn't exist)`,
* or a more significative **`PG::Error: ERROR:  no schema has been selected to create in`** when running `rake db:migrate`.

The culprit was this line in the `app.sql` dump file:

```
ALTER SCHEMA public OWNER TO postgres;
```

The schema's owner was changed to `postgres`, while Rails was trying to access it as another user (the database's owner).

I just changed `postgres` to match my database name, and it worked again. Be sure to check your schema's owner if you meet these issues!
