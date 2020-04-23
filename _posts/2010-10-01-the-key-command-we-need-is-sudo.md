---
title: Automatic backup of MySQL database
layout: post
categories: linux
permalink: 2010-10-the-key-command-we-need-is-sudo.html
date:   2010-10-02 
---

To create a backup of a mysql db, the command we need is the following;

```bash
sudo mysqldump -u UserName -p --all-databases > /path/to/file.backup
```

The `--all-databases` flag will back up everything inside of the MySQL instance. If you don't want to backup everything, specify the database name(s) instead of backing up everything.

The result of mysqldump is a series of SQL commands that contains both the structure and contents of the database.

**How to Restore**

```bash
mysql -u UserName -p 

mysql > create database name_of_name_database
mysql > use name_of_new_database
mysql > .\file.backup
```

Some extra things you can do to make this a bit more robust.

1. Put this command in a cron job
2. Refine by gzipping redirected text-output
3. Refine further by adding time stamp to the backup files
4. Refine by setting up Rsync to work with this backup approach
5. I could actually put backup file inside DropBox, then I won't need rsync. However, mostly likely, if the installation is enterprise grade, chances are the Dropbox code won't be an option
 