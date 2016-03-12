---
layout: post
title:  "Utilize PHP and MySQL to Build a Custom Forum (Part 3)"
date:   2014-01-14
---

## Part 3: Creating the Layout

There are certain elements of our website that will need to be included on several different pages and also will remain relatively unchanged on each. We could copy and paste these code elements but that would be time consuming, inefficient, and hard to update if it needs changed. It is best practice when programming to not repeat yourself. In our forum, we will create files that contain this code that needs to be repeated and when a page needs to use this code, we will use includes to inject the code needed into the file.

There are two main files that will create the layout of our website: 

1. The Header
2. The Footer

### The Header File:

The header file contains information required by the browser to tell it what type of content to expect and also this is where you will include any stylesheets (CSS) and javascript that you may need.

We create a basic header file below:

~~~ html
<!--- header.html --->

<!--- Declare Document Type--->
<!DOCTYPE html PUBLIC "-//W3C/DTD XHTML 1.0 Transitional//EN"
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">

<head>
	<!--- defines character set --->
    <meta http-equiv="content-type" content="text/html; charset=utf-8" />
	<!--- links to the the stylesheet --->
    <link rel="stylesheet" type="text/css" href="../css/style.css"  />
	<!--- sets title --->
    <title>MyForumSite</title>

</head>
<body>
~~~


###The Footer File

The footer file may display additional information that we would like displayed on every page such as the time, copyright information, or navigation links. 

We create the basic footer file below:

~~~ html

<!--- provide info on who created the site --->
<div class="footer_div text-center">
    <p class="created_by">Created by James Aten</p>
</div>

<!--- close body tag --->
</body>

<!--- close html tag --->
</html>

~~~

Our footer simply contains info about who created the site, closes the _body_ tag, and closes the _html_ tag.

### Layout Conclusion

We now have created a header and footer and file. These files will be included in most of our other files and the content for our site will be sandwiched between the header and the footer content. This allows us to easily modify part of the header or footer file and have this change be reflected throughout the site. It also allows us to provide one stylesheet to the header and have this determine the style for the entire website rather than having an individual stylesheet necessary on each page. This makes future management and updating easy because we only need to modify one file rather than several seperate stylesheets.

This layout structure provides efficiency, ease of management, and easy updating to our basic layout code. 

We now have a layout setup and are ready to create the settings files which will allow us to configure and connect to our database. 

Move onto Part 4 of this series to learn how to configure and connect to the MySQL database we created

[Part 4 - Configure and Connect to the Database]({% post_url 2014-01-16-utilize-php-and-mysql-to-build-a-custom-forum-part-4-configure-and-connect-to-the-database %})