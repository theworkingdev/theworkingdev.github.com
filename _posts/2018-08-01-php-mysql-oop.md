---
title: php mysqli oop
layout: post
categories: php
permalink: /2018/08/php-mysqli-oop.html
date:   2009-08-01 21:14:28 +0800
---

Start by connecting to the database

```php
define("DBSERVER" , "localhost");
define("DBUSER" , "dbuser");
define("DBPASS" , "dbpassword");
define("DBNAME" , "dbname");

global $cn;
$cn = new mysqli(DBSERVER,DBUSER,DBPASS,DBNAME);

if(mysqli_connect_errno()) {
  die("DBCONNECT | database connection failed" . mysqli_connect_error());
}
```

Fetch just the one record from the database

```php
$query = "SELECT * FROM tbousers WHERE idno=11";
result = cn->query($query);
row = result->fetch_assoc();

var_dump($row);
``` 

To get more than one record, use the following code

```php
$query = "SELECT * FROM tbousers";
result = cn->query($query);
row = result->fetch_assoc();

while(row = result->fetch_assoc()){
  var_dump($row);
}
```

If you want to store the results you got into a an associative array, use these codes

```php
$storage = array();
$query = "SELECT * FROM tbousers";
result = cn->query($query);
row = result->fetch_assoc();

while(row = result->fetch_assoc()){
  array_push(storage, row);
}

var_dump($storage);
```