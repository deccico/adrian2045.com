+++
title = "Concurrency in Hudson"
date = "2010-08-25"
description = "How to organize and avoid concurrency problems when implementing complex projects in Hudson?"
tags = ["hudson","jenkins","continuous integration","groovy","snippet"]
thumbnail = "images/posts/concurrency-294x300.jpg"
featured = false
codeLineNumbers = true
codeMaxLines = 50 # Maximum number of lines to be shown by default across all articles.
+++

How to organize and avoid concurrency problems when implementing complex projects in Hudson?
<!--more-->

![:left](/images/posts/ci_cycle.png)

When your Hudson projects are split in multiple jobs that are chained to each other, how do you avoid concurrency issues in Hudson?

If you have a lengthy process, it will be a good practice to split it in multiple tasks, having each calling the next one, like on the (rather simplified) left image.

Problem number one: How to avoid Clean and Build (for instance) to run at the same time?

As (almost) always, “you have a plugin for that” in Hudson. This time is [Locks and Latches](https://web.archive.org/web/20140527100803/http://wiki.hudson-ci.org/display/HUDSON/Locks+and+Latches+plugin), which will let you create a lock, shared by all the jobs you want. Only the job that has the lock will be executed.

So far, so good, but, where is the problem?

As you may guess, Clean could be called multiple times, while Build, Publish, or the same Clean are executed. How could this happen? If you trigger Clean with a SCM (like SVN) this could be an expected situation, as you have no way to prioritize the jobs in Hudson. This is explained in bug [833](https://web.archive.org/web/20140527121346/http://issues.hudson-ci.org/browse/HUDSON-833).

The following Groovy script checks if there is any other job waiting for an executor or being executed. This effectively prevents Clean to destroy the others jobs data. (Note: you will need the Groovy plugin and then create a “system Groovy script”.)

```groovy
//Jobs currently building or in the queue
jobs = hudson.model.Hudson.instance.items.findAll{job -> job.isBuilding()} + hudson.model.Hudson.instance.items.findAll{job -> job.isInQueue()}
 
//Keep only the names from the collected job objects
jobs = jobs.collect{x -> x.name}
 
//get current job name
currentJob = Thread.currentThread().executable.toString().split()[0]
 
//cut current job from the jobs list
jobs -= currentJob
 
//decide if there is any running (or waiting) job
res = true
if (jobs.size()  > 0)
{
    res = false
    println "The following jobs are being executed or waiting for an executor"
    jobs.each{println it}
}
else
{
    println "Concurrency OK"
}
 
return res
```

In case you have other jobs in your Hudson instance that have nothing to do with our “Clean -> Build -> Publish” project, you can restrict the concurrency checking to our project. 
In order to do that we will need to implement this small modification in our Groovy script:

```groovy
//Jobs currently building or in the queue
jobs = hudson.model.Hudson.instance.items.findAll{job -> job.isBuilding()} + hudson.model.Hudson.instance.items.findAll{job -> job.isInQueue()}
 
//Keep only the names from the collected job objects
jobs = jobs.collect{x -> x.name}
 
//list of jobs that belong to the same project
project_jobs = ["Compile", "Publish"]
 
//cut current job from the jobs list
jobs = project_jobs.intersect(jobs)
 
//decide if there is any running (or waiting) job
res = true
if (jobs.size()  > 0)
{
    res = false
    println "The following jobs are being executed or waiting for an executor"
    jobs.each{println it}
}
else
{
    println "Concurrency OK"
}
 
return res
```

![](/images/posts/concurrency-294x300.jpg)
