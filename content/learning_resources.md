---
layout: default
title: Learning Resources
---

# Udacity 
[Intro to Hadoop and MapReduce](https://www.udacity.com/course/intro-to-hadoop-and-mapreduce--ud617)

This course provides a great overview of the Hadoop ecosystem.  Big data definitions and problems are covered,as well as commands for running jobs, and creating mappers/reducers in Python.  
A virtual machine can be downloaded that simulates the Hadoop environment, and allows the user to run MapReduce jobs, without having an actual cluster in place.
Note:  These commands work on the Udacity, Hadoop image

[Course Overview Video](https://www.youtube.com/watch?v=44K_bzTL_SM)

[Big Data Wiki](https://en.wikipedia.org/wiki/Big_data)

## Commands/Notes
**Testing mapper or reducer locally:**

It is a good idea to take a small sample of the data and test mapper/reducer.
Take first 50 lines of test file and send to testfile.txt

    head -50 ../data/purchases.txt > testfile

Next, pipe testfile.txt to mapper.  This will print mapper output to stdout.

    cat testfile | ./mapper.py

If that works correctly, try the mapper and reducer.  We will also sort between map and reduce.

    cat testfile | ./mapper.py | sort | ./reducer.py

After completing the local test, run the job on the cluster/virtual machine.

FORMAT: hs mapper reducer inputDirectory newOutputDirectory

    hadoop mapper.py reducer.py myinput output