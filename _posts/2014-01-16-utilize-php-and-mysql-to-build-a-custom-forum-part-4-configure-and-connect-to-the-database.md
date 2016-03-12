---
layout: post
title:  "Utilize PHP and MySQL to Build a Custom Forum (Part 4)"
date:   2014-01-16
---

## Part 4: Configure and Connect to the Database

In [Part 2 - Setting Up the Database]({% post_url 2014-01-07-utilize-php-and-mysql-to-build-a-custom-forum-part-2-setting-up-the-database %}) we setup our database so that it is structured correctly to store the information we are going to need.

In this part, we are going to use PHP to configure and connect our website to the database.

first we create a file called _mysqli_connect.php_

~~~ php

<?php
//mysqli_connect.php

// Set the database access information as constants

DEFINE ('DB_USER', 'your_username');
DEFINE ('DB_PASSWORD', 'your_password');
/*
 *usually called 'localhost' and has a *default TCP/IP port of 3306 so 
 *therefore you would use the value 'localhost:3306' in place of 'your_host'
*/
DEFINE ('DB_HOST', 'your_host');
//this is the name you gave to your database
DEFINE ('DB_NAME', 'your_database_name');

// Connect to Mysql using constants and table name
$dbc = @mysqli_connect (DB_HOST, DB_USER, DB_PASSWORD, DB_NAME) OR die ('Could not connect to MySQL: ' . mysqli_connect_error() );

//if connection fails, trigger error
if(!$dbc)
{
    //try to connect without the database setup yet
    $dbc = mysqli_connect(DB_HOST, DB_USER, DB_PASSWORD);
    //Check if connection worked
    if(!$dbc)
    {
        trigger_error('Could not connect to MySQL: ' . mysqli_connect_error());
    }
    else
    {
        $q = "CREATE DATABASE '$db_name'";
        mysqli_query($dbc, $q);
    }

}
//connection was successful
else
{
    // Set encoding
    mysqli_set_charset($dbc, 'utf8');
}
~~~

We first define our mysql configuration settings as constants so we don't need to retype them all the time throughout this script.

Next we attempt to establish a connection to the database using _@mysqli_connect_ and if it is successful, it returns TRUE otherwise it returns FALSE.

We then check to see if _$dbc_ is TRUE or FALSE. 

If it is false, we attempt to connect without the database set. If it still does not connect, we trigger an error, if it does connect, we then create the database using the database name we defined.

If it is _$dbc_ is initially TRUE, we define the character set as _utf-8_ using _mysqli_set_charset($dbc, 'utf-8')_

Run this code and test whether you are able to connect. If you are able to connect, great! if not, check the values you provided to the constants for any errors.

### Conclusion

In this part we connected to the database using PHP, this a great step in creating a working forum as we will be using this connection often to access information stored in the database.

If you connected successfully, move to the next part where we create the homepage!

[Part 5 - Configure and Connect to the Database]({% post_url 2014-01-16-utilize-php-and-mysql-to-build-a-custom-forum-part-4-configure-and-connect-to-the-database %})