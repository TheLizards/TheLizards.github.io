---
layout: post
title:  "Utilize PHP and MySQL to Build a Custom Forum (Part 2)"
date:   2014-01-07
---

## Part 2: Setting up the Database

When you a builder is creating a house, he usually begins by building the foundation from the bottom up. I take the same approach when creating a website by first creating the database where all of our data will be stored and organized.	

### Creating the Database:
First you need to login to MySQL via either:

- the Command-Line
	- To connect through the Command-Line use this [guide](http://dev.mysql.com/doc/hurefman/5.5/en/connecting-disconnecting.html)
- A GUI (Graphical User Interface) for MySQL
	- If you are uncomfortable with the command line or prefer a GUI, the  the most common MySQL GUI software are listed below:
		- Windows: [MySQL Workbench](https://www.mysql.com/products/workbench/)
		- Mac: [Sequel Pro](https://www.mysql.com/products/workbench/)
		- Linux: If you are on linux, you probably are fine just using the command line but if not, just use [MySQL Workbench](https://www.mysql.com/products/workbench/)
Once we are logged into MySQL, we need to create the database. We will call our database _myforumsite_. 

In MySQL issue the following command to create a myforumsite database:

~~~ sql
CREATE DATABASE myforumsite;
~~~

Then we need to instruct MySQL that we are going to use this database.

~~~ mysql 
USE Myforumsite;
~~~

###Creating the Tables

Next we need to begin creating the tables for our database.

Let's describe our forum below:
- The forum will have several **categories** where discussion can take place
- The **users** should be able to create **threads** in a category
- Other users will then create **posts** in these threads

This helps us to see that we will need 4 tables to begin:

1. users
2. categories
3. threads
4. posts

#### The Users Table

We will create the users table first using the following mysql command:

~~~ sql
CREATE TABLE `users` (
  `user_id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `username` varchar(30) NOT NULL,
  `pass` char(40) NOT NULL,
  `email` varchar(60) NOT NULL,
  `user_level` tinyint(1) unsigned NOT NULL DEFAULT '0',
  `active` char(32) DEFAULT NULL,
  `date_created` datetime NOT NULL,
  PRIMARY KEY (`user_id`),
  UNIQUE KEY `username` (`username`),
  UNIQUE KEY `email` (`email`),
  KEY `login` (`username`,`pass`)
) ENGINE=InnoDB  DEFAULT CHARSET=utf8;
~~~

Since this is our first table, I will walk you through it line by line so you can understand what is going on. If you already understand, you can execute this command and skip to creating the next table.

Lets go line by line and figure out what is happening in this command:

~~~ sql 
CREATE TABLE `users`(
~~~
- Create a table with the name _users_

~~~ sql 
`user_id` int(10) unsigned NOT NULL AUTO_INCREMENT,
~~~
-  The first column in the users table will be called _user_id_
- _int(10)_ means that this field must hold an integer
	- The _10_ specifies the display width but this number does not constrain the range of values that can be stored
	- Display width is only applicable when using zerofill, which we are not
- _unsigned_ means that this column will always hold a positive number
- _NOT NULL_ means that this field cannot be empy
- _AUTO_INCREMENT_ means that each time a record is inserted, the _user_id_ increments by 1

~~~ sql 
`username` varchar(30) NOT NULL,
~~~

- The second column in the users table will be called _username_
- _varchar(30)_ means that this field will hold text
	- the _30_ specifies the max number of characters allowed in the _username_
- _NOT NULL_ means that this field cannot be empty

~~~ sql
`pass` char(40) NOT NULL,
~~~

-  The third column in the users table will be called _pass_
-  It will store the user's password
- _char(40)_ means that this field will hold text
	- the _40_ specifies the number of characters allowed in the _pass_
- _NOT NULL_ means that this field cannot be empty

~~~ sql
`email` varchar(60) NOT NULL,
~~~

-	The fourth column in the users table will be called _email_
-	It will store the user's email address
- _varchar(60)_ means that this field will hold text
	- the _60_ specifies the max number of characters allowed in the _email_
- _NOT NULL_ means that this field cannot be empty


~~~ sql
`user_level` tinyint(1) unsigned NOT NULL DEFAULT '0',
~~~

-	The fifth column in the users table will be called _user_level_
-	It will store the user's authorization level
- _tinyint(1)_ means that this field will hold an small integer
	- The range of integer values that can be stored in _tinyint_ are:
		- -128 => 127 (Signed)
		- 0 => 255 (Unsigned)
	- The _1_ specifies the display width but this number does not constrain the range of values that can be stored
- _unsigned_ means that this field can only hold 0 or positive integers
- _NOT NULL_ means that this field cannot be empty
- _DEFAULT '0'_ means that if this field is NOT given a value, it will result in the field storing the value 0

~~~ sql
`active` char(32) DEFAULT NULL,
~~~

-  The sixth column in the users table will be called _active_
-  It will specify whether the user has activated their account yet
- _char(32)_ means that this field will hold text
	- the _32_ specifies the number of characters allowed in the _pass_
- _DEFAULT NULL_ means that if this field not given a value, it will reslt in the field storing the value 0

~~~ sql 
`date_created` datetime NOT NULL,
~~~

- the seventh column in the users table will be called _date_created_
- it will specify the date and time that this row was created
- The _datetime_ type is used for values that contain both date and time parts.
	- MySQL retrieves and displays DATETIME values in 'YYYY-MM-DD HH:MM:SS' format
	- The supported range is '1000-01-01 00:00:00' to '9999-12-31 23:59:59'.
- - _NOT NULL_ means that this field cannot be empty

~~~ sql
PRIMARY KEY (`user_id`),
~~~

- _PRIMARY KEY_ means that we want to specify a field that will be a primary key
- _user_id_ specifies that we want the _user_id_ field to be the field is specified as the primary key
	- A primary key does not add any fields to the table
	- Most tables should have a primary key
		- Each table should only have one field specified as a primary key
	- The primary key constraint uniquely identifies each row in a table
		- A primary key must contain UNIQUE values
		- A primary key field cannot contain NULL values

~~~ sql
UNIQUE KEY `username` (`username`),
UNIQUE KEY `email` (`email`),
~~~

- _UNIQUE KEY_ means that we want to specify that a field MUST be UNIQUE
- You can have multiple UNIQUE constraints per table, but only one PRIMARY KEY constraint per table
- _username_ and _email_ specifies that we want these particular fields to be UNIQUE
	- once the UNIQUE KEY constaint is setup, if you try and insert a value into this column that already exists, you will receive an error and the value will NOT be inserted

~~~ sql
KEY `login` (`username`,`pass`)
~~~

-  _KEY_ is a synonym for _INDEX_ when using MySQL
	-  INDEXES allow you to query your MySQL tables quicker and more efficiently
	-  They are often used on columns which will need to be retrieved often
-  `login` (`username`,`pass`) means that we are creating a key name _login_ which indexes the _username_ and the _pass_ columns
-  _KEY_ unlike _PRIMARY KEY_ does not require the value to be unique in the specified column

~~~ sql
ENGINE=InnoDB  
~~~

- This statement specifies that the we will use the InnoDB storage engine for MySQL
	- The main alternative is called MyISAM, but all our tables will use InnoDB
	- To learn more about MySQL's storage engine options, read the [documentation](http://dev.mysql.com/doc/refman/5.5/en/storage-engines.html)
	
~~~ sql
DEFAULT CHARSET=utf8;
~~~

-  This simply defines the character set for the table to be _utf-8_

**Difference between _varchar_ and _char_**

As you may have seen, we use _varchar_ and _char_ several times when creating this table. Both are used to store text but how do they differ and when should you use each. I will try and summarize this below:

- CHAR
	-	Used to store character string value of fixed length.
	-	The maximum no. of characters the data type can hold is 255 characters.
	-	It's 50% faster than VARCHAR.
	-	Uses static memory allocation.
-	VARCHAR
	-	Used to store variable length alphanumeric data.
	-	The maximum this data type can hold is up to Pre-MySQL 5.0.3 :: 255 characters. In MySQL 5.0.3+ :: 65,535 characters shared for the row.
	-	It's slower than CHAR.
	-	Uses dynamic memory allocation.

So basically, when you know for sure that a field will contain text values of a fixed length use _char_ and when the text values length will vary, use _varchar_.

#### The Categories Table

Next we will create the table that contains the categories. This way your forum can have multiple categories where users can discuss related topics.

To create the categories table, use the following mysql command:

~~~ sql
CREATE TABLE `categories` (
  `category_id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `category_name` varchar(255) NOT NULL,
  `category_description` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`category_id`),
  UNIQUE KEY `category_name_UNIQUE` (`category_name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
~~~

The _categories_ table has 3 fields:

-  _category_id_
	-  This is the _PRIMARY KEY_ and will be a unique integer value that identifies this row
	-  Values must be unsigned (positive) integers
	-  This field must be _NOT NULL_
	-  Just like the _user_id_ in the last table, AUTO_INCREMENT means that each time a record is inserted, the _category_id_ increments by 1
-  _category_name_
	-  This is the name of the category
	-  It will contain a text value that does NOT exceed 255 characters
	-  It is _NOT NULL_
	-  We set a _UNIQUE KEY_ on this field since the _category_name_ must be UNIQUE
-  _category_description_
	-  This is a short description of the category that will be used to provide the users a guide to what topics are included in this category
	-  It will contain a text value that does NOT exceed 255 characters
	-  It defaults to _NULL_

#### The Threads Table

In a forum, a thread is a group of posts set up as a conversation among users. A thread will be created by a user and then other users will be able to create posts in this thread in order to discuss the topic of the thread.

To create the threads table, use the following mysql command:

~~~ sql
CREATE TABLE `threads` (
  `thread_id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `category_id` int(10) unsigned NOT NULL,
  `user_id` int(10) unsigned NOT NULL,
  `subject` varchar(255) NOT NULL,
  PRIMARY KEY (`thread_id`),
  KEY `user_id` (`user_id`),
  KEY `category_id` (`category_id`),
  CONSTRAINT `fk_threads_categories` FOREIGN KEY (`category_id`) REFERENCES `categories` (`category_id`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `fk_threads_users` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
~~~

The _threads_ table has 4 fields:

-  _thread_id_
	-  This is the _PRIMARY KEY_ and is just like the _user_id_ and the _categories_id_ in the previous tables
-  _category_id_
	-  Since each thread will belong to a category, we need to reference the _category_id_
-  user_id
	-  Since each thread will belong to a user, we need to reference the _user_id_
-  _subject_
	-  This field contains the topic of the the thread
	-  It will contain text but cannot exceeed 255 character

Since the _category_id_ and the _user_id_ are references to fields in other tables, we want a way of indicating this in our database structure. This is achieved by using Foreign Keys.

**Foreign Keys**

Since MySQL and most other web storage databases are "Relational Databases", they provide us with tools to structure our data and also to indicate how our data is grouped, organized, and related.

One of the ways to show how our data and tables are related and to enforce this relation is through foreign keys.

A foreign key is a reference to a primary key in another table. It provides us with a structure that clearly shows a relation between the tables and also helps us maintain "referential integrity". This means that we can enforce that if a value is in one table then it must also exist in the referenced table. If it does not, then the query will fail.

In our forum example, we create two foreign keys in our _threads_ table. 

The foreign key linking threads and categories

Since each thread belongs to a category, we use a foreign key to define and enforce this relation

~~~ sql
CONSTRAINT `fk_threads_categories` 
FOREIGN KEY (`category_id`) 
REFERENCES `categories` (`category_id`) 
ON DELETE CASCADE ON UPDATE CASCADE,
~~~

Lets go line by line again to see how a foreign key is constructed

~~~ sql
CONSTRAINT `fk_threads_categories` 
~~~
- _CONSTRAINT_ is a MySQL command that signifies that we want to create a constaint, or something that will enforce data integrity or relations
- `fk_threads_categories` is the name we give to this foreign key contstaint
	- The foreign key name must be unique for this database
	- the most commonly used standard for naming foreign keys is:
		- fk_[referencing table name]_[referenced_table name]
		- in this case the _threads_ table is referencing the _categories_ table

~~~ sql
FOREIGN KEY (`category_id`)
~~~

-  this specifies that we would like to create a foreign key on the _category_id_  field (the referencing field)


~~~ sql
REFERENCES `categories` (`category_id`) 
~~~

-  This line specified what table and column is being referenced
	-  The table being referenced is _categories_
	-  The column in _categories_ that is being referenced is _category_id_
		-  We reference _category_id_ because it is the primary key and uniquely identifies this category


~~~ sql
ON DELETE CASCADE ON UPDATE CASCADE,
~~~

- The last line states that if we _DELETE_  or _UPDATE_ the referenced row in the referenced table, this will _CASCADE_ to the referencing row in the referencing table
	- This ensures that there can never be a foreign key field that references a value in another table that does not exist

The foreign key linking threads and users

~~~ sql
CONSTRAINT `fk_threads_users` 
FOREIGN KEY (`user_id`) 
REFERENCES `users` (`user_id`) 
ON DELETE NO ACTION 
ON UPDATE NO ACTION
~~~

I am not going to break this down line by line since it is very similar to the foreign key linking _threads_ and _categories_. But below are the basics:

-  We create a foreign key named `fk_threads_users` since the threads table is referencing the users table
-  The referencing field is _user_id_ in the _threads_ table and it references _user_id_ in the _users_ table
-  Notice here that our rules _ON DELETE_ and _ON_UPDATE_ are different that with the categories foreign key
	-  In this foreign key we will not have a deletion or update of a row in the _user_ table trigger a deletion or update of a row in the _threads_ table
	-  Why?
		-  This is a matter of personal preference and is determined by the needs of your site.
		-  I chose not to have deletions and updates trigger a cascade in this case because in a forum, a user creates a thread and then other users create posts in that thread. If we allowed a deletion to cascade, not only would the deleted users threads be deleted, but the posts from other users would also be deleted. This could cause issues and upset other users whose post would now be gone.
 
To clarify foreign keys in concrete terms lets use our forum as an example:

- First we create two categories in our  _categories_ table named:
	-  cats
	-  dogs
-  We then want to create threads in these categories so that we can discuss these with other users
	-  We attempt to create a thread named "persian"  to discuss persian cats and this thread references the category "cats"
		-  This query works because there is a category named "cats", so it passes our foreign key constraints
	-  We attempt to create a thread named "bulldog" to discuss bulldogs and this thread references the category "dogs"
		-  This query works because there is already a category named "dogs" so it passes our foreign key constraints
	-  We attempt to create a thread called "goldfish" which discusses goldfish and this thread references the category "fish"
		-  This query fails because there is not a category named "fish", so our foreign key constraint fails.
		-  If we did not setup a foreign key, this query would have passed and been inserted into the threads table, but it would reference a category that does not exist. This could cause problems and should be avoided
	-  If one day we decide we hate "cats" and we only want our forum to have a "dogs" category, we can delete our "cats" category.
		-  If we have our foriegn key set to cascade on delete, then any threads that belong to the "cats" category would be deleted when we delete the "cats" category
			-  This is helpful because it automatically cleans up our database and prevents us from having "cat" related threads when we now hate "cats" and don't have a "cats" category
		-  If our foreign key was NOT set to cascade on delete, then any threads that belong to the "cats" category would be left intact.
			- This could cause issues because we hate "cats" and lack a "cats" category now, so our "cat" threads now reference a category that does not exist.

Finally we need to create a foreign key

Hopefully this helps you understand the basics of foreign keys better. As we progress in creating our forum, you will see that these foreign keys will help us maintain/manage our database and prevent invalid data from being entered.

#### The Posts Table

Our users should also be able to create posts a thread. Therefore we need a table to store these posts. Since each post will belong to a user and each post will belong to a thread, we will need to create foreign keys again to enforce these relations

To create the _posts_ table, use the following mysql command:

~~~ sql 
CREATE TABLE `posts` (
  `post_id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `thread_id` int(10) unsigned NOT NULL,
  `user_id` int(10) unsigned NOT NULL,
  `message` text NOT NULL,
  `posted_on` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`post_id`),
  KEY `thread_id` (`thread_id`),
  KEY `user_id` (`user_id`),
  CONSTRAINT `fk_posts_threads` FOREIGN KEY (`thread_id`) REFERENCES `threads` (`thread_id`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `fk_posts_users` FOREIGN KEY (`user_id`) REFERENCES `users` (`user_id`) ON DELETE NO ACTION ON UPDATE NO ACTION
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
~~~

If you have been following along and understand how to create the _categories_ and _threads_ tables, you should understand what is going on in creating the _posts_ table. Therefore I am not going to go line by line, but I will highlight a few things:

-  We create foreign keys for _thread_id_ and _user_id_ in order to maintain our relational integrity
-  The _message_ field will contain text and uses the MySQL datatype _text_
	-  The _text_ datatype can store a max of 65535 characters
	-  The _text_ datatype cannot be indexed or be used as a PRIMARY KEY
-  The _posted_on_ field defaults to _CURRENT_TIMESTAMP_ which is a MySQL function that automatically inputs the current date and time into the designated field

### Conclusion

We have now created our database and the tables required to store the data for our forum. In the next lesson we will create the layout for our forum.

Move onto Part 3 of this series to learn how to Create a Layout for your Forum

[Part 3 - Creating a Layout]({% post_url 2014-01-14-utilize-php-and-mysql-to-build-a-custom-forum-part-3-creating-the-layout %})





