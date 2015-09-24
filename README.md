# dokku-pg-plugin [![Build Status](https://img.shields.io/travis/Kloadut/dokku-pg-plugin.svg?branch=master "Build Status")](https://travis-ci.org/Kloadut/dokku-pg-plugin)

Project: https://github.com/progrium/dokku

**Warning: This plugin is under development and still only tested with the below dependencies**

## requirements

- dokku 0.4.0+
- docker 1.8.x

## installation

```shell
# on 0.3.x
cd /var/lib/dokku/plugins
git clone https://github.com/Kloadut/dokku-pg-plugin.git postgresql
dokku plugins-install

# on 0.4.x
dokku plugin:install https://github.com/Kloadut/dokku-pg-plugin.git postgresql
```

## commands

```
$ dokku help
    postgresql:console <db>                        Open a PostgreSQL console
    postgresql:create <db>                         Create a PostgreSQL container
    postgresql:delete <db>                         Delete specified PostgreSQL container
    postgresql:dump <db> > dump_file.sql           Dump database data
    postgresql:info <db>                           Display database informations
    postgresql:link <app> <db>                     Link an app to a PostgreSQL database
    postgresql:list                                Display list of PostgreSQL containers
    postgresql:logs <db>                           Display last logs from PostgreSQL container
    postgresql:restore <db> < dump_file.sql        Restore database data from a previous dump
```

## usage

Create a new DB:

```shell
dokku postgresql:create foo            # Server side
ssh dokku@server postgresql:create foo # Client side
```

The following will be output:

```
-----> PostgreSQL container created: postgresql/foo

       Host: 172.17.42.1
       User: 'root'
       Password: 'RDSBYlUrOYMtndKb'
       Database: 'db'
       Public port: 49187
```

Deploy your app with the same name (client side):

```shell
git remote add dokku git@server:foo
git push dokku master
```

Now you can run any of the following commands:

```shell
# Link your app to the database
dokku postgresql:link app_name database_name

# Inititalize the database with SQL statements
cat init.sql | dokku postgresql:create foo

# Open a PostgreSQL console for specified database
dokku postgresql:console foo

# Deleting databases
dokku postgresql:delete foo

# Linking an app to a specific database
dokku postgresql:link foo bar

# PostgreSQL logs (per database)
dokku postgresql:logs foo

# Database information
dokku postgresql:info foo

# List of containers
dokku postgresql:list

# Dump a database
dokku postgresql:dump foo > foo.sql

# Restore a database
dokku postgresql:restore foo < foo.sql
```
