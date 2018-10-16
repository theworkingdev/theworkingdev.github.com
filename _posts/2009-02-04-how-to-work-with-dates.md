---
title: PHP How to work with dates
layout: post
categories: php
permalink: /2009/02/php-how-to-work-with-dates.html
date:   2009-02-14 21:14:28 +0800
---

Your programs will deal with date and times some of the time. On some occassions, the date and time data will be inputs coming from an html view. These inputs will be in string format which means you need to transform them to the appropriate date/time format before you can persist them to the database. On other occassions,  you will read the date/time data from a database and then display them in the html view.

There are two functions that will be handy when you need to work with date and times, these are the `date` and `strtotime` functions.

### Handling date Inputs in a form

The code below will extract the data from the `inputdate` element sent from an html form.

```php
date = date('Y-m-d', strtotime(_POST["inputdate"]));
```

The `strtotime` function by itself may not be enough. You will need to use it in tandem with the `date` function; `strtotime` takes a date or time data in string format and converts it to a UNIX timestamp. This timestamp is an integer value which represents the number of seconds that has elapsed since January 1 1970 (UTC). This timestamp is also known as POSIX time or epoch time. 

The `date` function needs 2 parameters. The second parameter is a timestamp data, that was why we needed the `strtotime` function, and  the first parameter is a string that will dictate how the date information will be formatted. Here are some common usage of **date** and **strtotime**.

```php
echo date('Y-m-d', strtotime("now")); // 2015-05-09
echo date('m-d-Y', strtotime("now")); // 05-09-2015
echo date('l, M d, Y', strtotime("now")); // Saturday, May 09, 2015
```

There are many format characters that you can use, head over to the documentation page for a full listing, they are listed on references section below.

### Presenting date output to a View

```php
$query = "SELECT username, workdate, timein, timeout FROM attendance";

result = cn->query($query);
row = result->fetch_assoc();

while(row = result->fetch_assoc()){
  echo date('m-d-Y',$row['workdate']);
  echo date('H:i', $row['timein']);
  echo date('H:i', $row['timeout']);
}
 
```

The `H:i` time format will echo the time date in 24 hour format. If you need to display it in 12 hour format, use the `g:i a` formatter. 

```php
echo date('H:i', $row['timein']);   // 21:17
echo date('g:i', $row['timein']);   // 9:17
echo date('g:i a', $row['timein']); // 9:17 pm

```

### Formatting dates using mysql database functions

The PHP `date` function is very flexible and allows to do lots of things already. For most situations formatting a date variable using PHP functions will suffice. There might be situations when this approach can be tedious.

 ```php
$returnarray = array();
$query = "SELECT username, workdate, timein, timeout FROM attendance";

result = cn->query($query);
row = result->fetch_assoc();

while(row = result->fetch_assoc()){
  workdate = date('m-d-Y',row['workdate']);
  timein = date('H:i', row['timein']);
  timeout = date('H:i', row['timeout']);
  array_push(returnarray, array('workdate'=> workdate, ...));
}

echo json_encode($returnarray);

 ```

The code sample above reads the the attendance record, formats it and then aggregates each record into an associative before echoing  it so than an AJAX client may consume it. The only reason we parsed the columns is so that the date/time data can be properly formatted. If we didn't need to format it, the code could have been written like the sample below.

```php
$returnarray = array();
$query = "SELECT username, workdate, timein, timeout FROM attendance";

result = cn->query($query);
row = result->fetch_assoc();

while(row = result->fetch_assoc()){
  array_push(returnarray, row);
}

echo json_encode($returnarray);

```

In these situations, we can do the date formatting on the database level rather than on the application level. MySQL has built-in functions that can be used for date transformations.

```php
$returnarray = array();
$query = "SELECT username,
          DATE_FORMAT(workdate, '%d.%m.%Y'),
          DATE_FORMAT(timein, '%h:%i %p'),
          DATE_FORMAT(timeout, '%h:%i %p')
          FROM attendance";

result = cn->query($query);
row = result->fetch_assoc();

while(row = result->fetch_assoc()){
  array_push(returnarray, row);
}

echo json_encode($returnarray);

```

The issue with this approach is that the resulting code might not be portable because we did use a specific function of MySQL. This might not work for other databases.

The `DATE_FORMAT` function has quite a lot of format specifiers, you can find the details of the format specifiers in MySQL documentation page which is listed on the references section.

### References  

1.  PHP.net/manual [strtotime](http://php.net/manual/en/function.strtotime.php)
2.  PHP.net/manual [Date](http://php.net/manual/en/function.date.php)
3.  Dev.MySQL [Doc/Date and Time Functions](https://dev.mysql.com/doc/refman/5.5/en/date-and-time-functions.html#function_date-format)

