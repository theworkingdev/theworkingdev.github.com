---
title: Connect to database via mysqli procedural 
permalink: /2009/02/php-mysqli-procedural.html
categories: php
layout: post
---

Start by connecting to the database. PHP is a server side scripting language and heavily dependent on the http protocol. The  request/response nature of http means that whatever data or information you created on previous requests, will not matter in future request. Everytime you need to do something that involves a database operation (insert, delete, update or select), you need to make a connection to the database before you can do anything else.

```php
// filename db.php
<?php
define("DBSERVER" , "localhost");
define("DBUSER" , "theuser");
define("DBPASS" , "thepassword");
define("DBNAME" , "thedb");

global $cn;
$cn = mysqli_connect(DBSERVER,DBUSER,DBPASS,DBNAME);

if(mysqli_connect_errno()) {
  die("DBCONNECT | database connection failed" . mysqli_connect_error());
}
?>

```

The code above is boilerplate, the usual practice is to write this code on a separate file e.g. an include file and "include" or "require" it before doing some other database handling codes like inserts or updates etc.

**How to fetch just one record** 

```php
<?php

require_once("db.php");

$query = "SELECT * from users;"
result = mysqli_query(cn, $query);

if(!result) die("db error " . mysqli_error(cn));

row = mysqli_fetch_assoc(result);
?>

```

Notice the **require_once** statement on the first line. We need to connect to the database first before we can proceed with fetching a record.

**How to fetch more than one record**  

```php
<?php
require_once("db.php");

$query = "SELECT * from users";
result = mysqli_query(cn, $query);

if(!result) die("db error " . mysqli_error(cn));

while(row = mysqli_fetch_assoc(result)) {
  fname = row['firstname'];
  lname = row['lname'];
  echo "{fname} {lname}";
}
?>

```

The **mysqli_fetch_assoc** will return an associative array rather than a basic array. You may find, in the course of writing CRUD applications, that associative arrays are more useful than basic arrays. It is more intuitive to read a code like **$fname = $row["firstname"]** rather than a code like **$fname = $row[0]**

There maybe times when you need to work with numeric arrays, for example, if you need to write a pretty generic code that can traverse any given result from **mysqli_fetch**. To work with numeric arrays, you can use the following code

```php
<?php

require_once("db.php");

$query = "SELECT * from users";
result = mysqli_query(cn, $query);

if(!result) die("db error " . mysqli_error(cn));

while(row = mysqli_fetch(result, MYSQL_NUM)) {
  fname = row[0];
  lname = row[1];
  echo "{fname} {lname}";
}
?>

```

**How to update a record** 

```php
<?php

require_once("db.php");

idno  = _POST['idno'];
fname = _POST['firstname'];
lname = _POST['lastname'];

$query = "UPDATE users SET firstname={$fname},
          lastname={$lname}
          WHERE idno={$idno}";

result = mysqli_query(cn, $query);
if(!result) die("db error " . mysqli_error(cn));
?>
 
```

The code above is a typical program that handles a form submission. the **fname**, **lname** and **idno** variables were extracted from the **$_POST** superglobal. The code is not very robust though because it will break if the html form sends data like O'Connel or  any other data that needs to be properly escaped first before saving to the database. Most of the time, when you allow the user to type the data which will be saved to a database, you must always ensure that the data is properly escaped. You can do that by using the **mysqli_real_escape_string** function.

```php
<?php

require_once("db.php");

idno   = mysqli_real_escape_string(cn, $_POST['idno']);
fname  = mysqli_real_escape_string(cn, $_POST['firstname']);
lname  = mysqli_real_escape_string(cn, $_POST['lastname']);

$query = "UPDATE users SET firstname={$fname},
          lastname={$lname}
          WHERE idno={$idno}";

result = mysqli_query(cn, $query);
if(!result) die("db error " . mysqli_error(cn));
?>

```

The **mysqli_real_escape_string** function takes two parameters. The first one is **connection** variable and the second parameter, obviously, is the actual string data that you would like to escape.

 