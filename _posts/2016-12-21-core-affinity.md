---
author: ameyjadiye
layout: post
title: "Core Affinity"
date: 2016-12-21 7:25
category : Linux, Java
comments: true
tags:
- java, linux
---


Core affinity _or_ Process affinity *always* playes a great roll for abtaining optimal latency if your application runs with limited threads.

Ideally application must keep limited threads and reuse it and many application does it unless immature programmer has done the coding, many multithreading applications frameworks also keeps the threadpool to reuse them, problem usually happpens when huge ammount of data comes in to the syatem and each threads have to do lot of work without being idle, lot of work brings lot of paging and context switching which is a costly operation, you can use ``` sar -w 5 5 ``` to check your context swiching.

I'm working with java application which works in burst mode for some hours and  then works normal throughout a day, in that 1-2 hour  huge context switching happens which decrease  overall performance, i created small script which can divide threads evenly in all availabel cores and apply affinity to them so no context-switching happens which increases performance tremendously.

Below script takes pid of your java process and assigns core to each thread, there are lib which do the same from within java process but its way more hectic and i see  with shell its  waymore easy ;)

```bash
#!/bin/bash

usage ()
{
  echo 'Usage : affinity <java_pid>'
  exit
}

if [ "$#" -ne 1 ]
then
  usage
fi


_index=0
_cores=`nproc`


for nid in `jstack $1 | grep nid  | cut -d' ' -f6 | grep nid | cut -d'=' -f2 | cut -d'x' -f2`
do 
	taskset -p -c $(($_index % $_cores)) $((16#$nid)) 
	((_index++))
done

```

source is here https://github.com/ameyjadiye/core-affinity , you can always provide improvements by pull req.

Happy Coding .. :)
