---
layout:default
title:Hadoop Streaming
---



    # User specific aliases and functions
    run_mapreduce(){
        yarn jar /usr/hdp/2.4.0.0-169/hadoop-mapreduce/hadoop-streaming-2.7.1.2.4.0.0-169.jar -mapper $1 -reducer $2 -file $1     -file $2 -input $3 -output $4
    
    }
    alias hs=run_mapreduce
