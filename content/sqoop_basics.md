---
layout: default
title: Sqoop Basics
---
#Using Sqoop

Sqoop is a tool to migrate data from a MySQL database management system into the Haddop file system (HDFS). It works much like any other tool installed wit Hadoop, and requires a command with arguments.

##Using Sqoop on Oracle 12c
An example command, run on our system looks like this:

   sqoop import --verbose --connect "jdbc:oracle:thin:hadoop/hadoop@//numbertwo:1521/orcl" --username hadoop -P --table TYLER.TWEETS

###Command Explained
_Import_ means that the action you want sqoop to complete is to import data into the Hadoop File System.

The _--verbose_ option was for debugging purposes, to find the error messages as we tried to make it work with our database instance.

The _--connect_ command is for connecting to the database.

The part in the string is our database's connect string, what sqoop will use to connect to the database and migrate the data.

_--username_ and _-P_ are for giving sqoop access to the database as a user with the right permissions on the specific table that needs to be imported. _-P_ will make sqoop ask for the password in the command window as a prompt, this is more secure than entering the password into the command itself.
  

