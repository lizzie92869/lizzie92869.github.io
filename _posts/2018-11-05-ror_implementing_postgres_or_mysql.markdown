---
layout: post
title:      "RoR: implementing Postgres or MySQL"
date:       2018-11-05 17:29:19 +0000
permalink:  ror_implementing_postgres_or_mysql
---


I learn to use Ruby on Rails with the default settings. So I learned to work with SQLite3 without even really understand what it was and what were my other options. 
Then I tried to deploy one of my applications on Heroku and I couldn't keep my database as it because Heroku doesn't support SQlite3. After some research I migrated to Postgres. 
More recently I met with a company who is using MySQL and tried to learn about the differences and how to implement such a relational database. Having implemented those different databases, I thought it would be usefull to explain this to a beginner trying to navigate through those concepts. 

**SQL vs SQLite3/MySQL/Postgres/Oracle vs MySQL workbench/pgAdmin**

quick definitions:

**SQL**: Structured Query Language - this is the language used to access and manipulate the database
**SQLite3/MySQL/Postgres/Oracle**: relational database - this is the database itself (collection of data) + the database management system (method for accessing and manipulating the data)
**workbench/pgAdmin**:  Graphical User Interface (GUI) allowing to have a nice visual representation of the relational database (optional). Workbench is for MySQL and pgAdmin for Postgres.

To learn about Postgres and MySQL I followed courses on Udemy and as good as they are I learned that you do not need those to work with Postgres and MySQL if you already know how SQL queries work. 
The training on those tools focus on the CRUD that we are writting in the SQL language to access and manipulate data in the database. For example doing: SELECT * FROM users WHERE user_id=2; 
So if you are already familliar with how to create, read, update and delete data in SQL, skip the courses. You can get a refresher there: https://www.w3schools.com/sql/. You can go through all the methods by the left menu. 

So what's the difference in implementing SQLite3, MySQL and Postgres? On a practical point of you, how do I choose to implement a MySQL database or Postgres database instead of the default sqlite3? 
# Implementing a MySQL database from the beginning of the project
- Make sure you have MySQL installed 
To see if you have it and know which version try the command line: which mysql. 
If you don't have it, install it from this link: https://dev.mysql.com/downloads/mysql/

- Generate your new app with this: rails new ProjectName -d mysql
this provide you with a MySQL database, adding for you the *gem 'mysql2', '>= 0.3.18', '< 0.6.0'* in your gemfile
Now you can create your migration the same way you were used to because ActiveRecords is going to convert to the right synthax and the right data types for MySQL. Nice, isn't it? 
# Implementing a Postgres database from the beginning of the project
- Make sure you have MySQL installed 
To see if you have it and know which version try the command line: which mysql. 
If you don't have it, install it from this link: https://dev.mysql.com/downloads/mysql/

- Generate your new app with this: rails new ProjectName -d postgresql
this provide you with a MySQL database, adding for you the *gem 'pg', '>= 0.18', '< 2.0'* in your gemfile
Now you can create your migration the same way you were used to because ActiveRecords is going to convert to the right synthax and the right data types for MySQL.
# Converting a SQLite database to a MySQL database (with no data in the db to save)
- Remove the sqlite3 gem

- Add the folowing gems:
```
gem 'mysql', '~> 2.9', '>= 2.9.1'
gem 'mysql2'
```

- In config/database.yml replace:

```
default: &default
  adapter: **mysql2**
  timeout: 5000
  encoding: utf8
  reconnect: false
  database: **name_db**
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  host: localhost

development:
  <<: *default
  database: **name_db_development**

<<: *default
  database: **name_db_test**

production:
  <<: *default
  database: **name_db_production**
```
	
For the name_db remove any extensions

- Remove the .sqlite3 file from the db folder 

- Run: bundle install

- If issue to install try
`brew upgrade openssl`
if asked run:
`xcode-select --install`

- Run: 
```
rake db:create
rake db:migrate
```

# Converting a SQLite database to a Postgres database (with no data in the db to save)

- Remove the sqlite3 gem

- Add the folowing gems:
`gem 'pg'`

- In config/database.yml replace:

```
default: &default
  adapter: **postgres**
  timeout: 5000
  encoding: utf8
  reconnect: false
  database: **name_db**
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  host: localhost

development:
  <<: *default
  database: **name_db_development**

<<: *default
  database: **name_db_test**

production:
  <<: *default
  database: **name_db_production**
```
	
	
- Remove the .sqlite3 file from the db folder 

- Run: `bundle install`

- Run: 
```
rake db:create
rake db:migrate
```



