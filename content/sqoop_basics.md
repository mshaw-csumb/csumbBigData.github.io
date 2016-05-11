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
The _--verbose_ option was for debugging purposes, to find the error messages as we tried to make it work with our database instance.

_--connect_
  

