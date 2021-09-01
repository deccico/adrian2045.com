+++
title = "Introduction to Continuous Integration"
date = "2010-10-10"
description = "Sometimes you got the right people, a good language and frameworks, but working in a project is a mess and nobody knows what will happen when you update the code and try to compile or running any functionality."
tags = [
    "continuous integration",
    "agile",
    "hudson",
    "jenkins"
]
thumbnail = "/images/posts/cicd.png"
featured = true
+++

Applying Continuous Integration best practices can make your team deliver more functionality in a fraction of time.
<!--more-->

![CI/CD](/images/posts/cicd.png "Continuous Integration")[^1]

[^1]: Image Source: https://www.padok.fr/hs-fs/hubfs/Images/Blog/continuous-integration-diagram.png?width=600&name=continuous-integration-diagram.png

# Introduction to Continuous Integration

Sometimes you got the right people, a good language and frameworks, but working in a project is a mess and nobody knows 
what will happen when you update the code and try to compile or running any of its functionality.

When you are working in a team that manages big systems trying to hit deadlines. There are certain issues that you simply
have to avoid. Some years ago, tired of facing the same problems, I started researching Continuous Integration techniques. 
This was the key to achieve a new level of software quality.

# What problems can be solved?

## Integration used to be an unpredictable process.

I remember one of my first projects at university (Algorithms III). I coded a chess game with a class mate. 
After one or two weeks we finished all the code. Unfortunately it took twice as much time to make both pieces of the system to run. 
It was only after creating some unit and system testing (which we also learned in that subject) the system worked. 
Similarly, the same situation happens in big software companies. Individual portions of a system often break all or some 
functionalities, sometimes very frequently.

## Merge code from different sources lead to difficulties finding where issues originate.

Sometimes we realize that there is a flaw in the code. Was it because an external component or any house-made code? 
In any case, no one should waste time looking for a (usually) preventable problem.

## Untouchable code.

Ugly code survives in the project because of fear of breaking something. This is also a common issue, specially in old systems 
where you can find different “layers” of functionality. These layers don’t follow an MVC or any other pattern but rather the 
age of the code. If any new functionality has to be added, usually developers will use a hack. 
If any portion of code deserves a redesign or to be refactored, none of this will happen, because of fear of breaking some 
functionality within our bloated code. If the code is working then “don’t touch it” is the common phrase that names all the 
passed opportunities to enhance a system, or even better, to learn something new from breaking it.

Clearly, if we have more information of exactly what remains working correctly after a modification, then we could make changes
with much more confidence.

# More problems

## svn update / p4 sync / git fetch. After an update, the system started failing!

It doesn't matter what software configuration manager (scm) tool you are using. If there is no automatic way to ensure 
the quality of the software -and more importantly- the quality of the artifacts you are downloading from the repository 
then you could end up with compilation or running errors after updating. The use of a scm tool is the proper way of 
tracking changes and rolling back to a previous state. But if you don’t get automatic information of exactly what is 
happening on each revision, you could end up spending a lot of time trying to find out the true state of the last code version.

## No one noticed that XYZ functionality has stopped working weeks ago.

Stealth like a ninja, a bug could be added without being noticed for a long time. This could ruin the trust over a system. 
Usually the solution is to add some layers of tests around the code. But unless they are regularly executed without 
relying on previously built artifacts, special environment configurations or data schemas, they can’t be entirely helpful. 
A Continuous Integration system could fix this situation.

## No one knows what change broke the code.

If we already have a repository where we commit and get updates, a Continuous Integration system could be helpful by tracking 
and testing every single versions from our system.

## Delays. One hour task ends up taking half a day.

Usually you feel (and name) something as an easy and quick task. But very often, the code manages to surprise you, taking down 
your estimations and your boss’s expectations. But software shouldn't be unpredictable. You can simply let a third party 
(a CI system) test every single commit from the repository.

# Solutions

## Catch the problems as soon as they happen.

A proactive attitude against this kind of problems is not just to say “we have to be very careful with these errors” like I heard 
many times. It doesn't matter how careful you are crossing an avenue that lacks a semaphore. We should accept that human errors 
will always happen, but with the right process we will address most of these issues.

## Treat integration as a non-event.

Working in a team or with external dependencies means that sometimes we will have to merge different pieces of code. 
Instead of waiting to make it at some point in the future we should treat integration as a “continuous” event. 
This is where the name comes from.

## Origin of Continuous Integration

The first references to Continuous Integration techniques were described by Grady Booch in his book 
“Designing Strategies for Object Technology” (1997)

 > In the context of continuously integrating a system’s architecture, establish a projects rhythm by driving to 
 > closure certain artifacts at regular intervals
    
 > Rather than set aside a period of formal testing at the end of the life-cycle, the object-orientated life cycle 
 > tends to integrate the parts of software (and possibly hardware) at more regular intervals…
    
 > ..at regular intervals, the process of continuous integration yields executable releases that grow in 
 > functionality each release…

But it was with the raise of Extreme Programming (XP) that this and other techniques took force.

## From The Agile Manifesto

 > Our highest priority is to satisfy the customer through early and continuous delivery of valuable software…

The first of the principles in the Agile Manifesto points to the process of software development as a flow for increasing 
value for the customer. I don’t imagine a better way of accomplish this but with the use of a Continuous Integration system.

## From XP (extreme programming) rules.

* Continuous integration
* Design improvement
* Small releases

These three XP rules are group together as “Continuous Process” pointing not only to the use of Continuous Integration, 
but to some practices around it, like taking the opportunity to refactor and redesign when making a correction. 
For instance, if our CI server notified an error, we could maybe refactor the solution in a general function used wherever 
makes sense, or if we see an error in a lengthy complex code or class, maybe we could split it in smaller, cohesive pieces. 
Of course there are other not so obvious opportunities to redesign and XP tell us to do it whenever it is clear that we are 
adding value to the system. After we make our changes, our CI tool will let us know the post-refactor health status of our system.

Another good technique in a symbiotic relationship with CI is the habit of making small “and frequent” releases to avoid looking 
for errors in a lengthy bunch of lines and files of code that comprises the failing change-set.

# So, what is Continuous Integration?

At this point, we surely have a good picture of what Continuous Integration is. Lacking a proper Computer Science taxonomy, what I 
collected as the main principles of this practice include:

## Team members integrate their work early and often

As the name suggests, there should be a continuous flows of commits to the code repository. Usually a daily commit per developer 
is a good target to start with.

## Each integration is verified by an automated build

There should be something acting as the Continuous Integration server that takes the last state of the code and builds it in 
an independent environment. This process should also include running as many tests as possible as we get a better knowledge 
of the state of the system.

## Is much more than installing a tool

As always, there are no silver bullets. Implement Continuous Integration is much more than choosing and installing a CI server. 
Of course, such tool could be of great help, but we need to be sure that we are following the practices to let the process 
provide real value for our team and our project.

# Best practices while implementing a CI server

There are some simple practices that will let you get the most of our CI server like:

## Including all elements in the repository.

Without controlling all dependencies in a single place, we will never be able to reliably deploy a new version in our server. 
We need to track all changes that could affect any functionality.

## Tests. Included within the build process.

If we only compile the system in the CI server, we will be missing important information and never notice when the software 
functionality stops working properly. Today we have many xUnit frameworks and auto-testing tools that are capable of adding 
valuable information to the integration process.

## Use an integration machine. In an isolated and virgin environment.

In a perfect situation, a single script should set up all the environment variables, servers, schema instances, 
libraries and so on. Automatically and from scratch. In this “big bang” way of working, our system will be far more reliable 
than any other that needs special screws and screw drivers to be adjusted. 
As a positive side effect there won’t be any effort to set up the environment for new hires or deploying a new 
instance. The team can instead focus in adding new value for the business.

## Frequent submits.

Subdivide the work to find bugs sooner and communicate the team what we are doing.

## Fast! Every minute counts. Keep the build fast to optimise everybody’s time.

The whole process should be prepared to give feedback as soon as possible. If we find that, for example, 
some tests are taking too long, or that a compilation from scratch takes more than an hour, we could configure a nightly 
and a daily integration. While the first one would be working from scratch, the daily integration should catch most of the 
errors in just a few minutes. We should wisely try to apply the Pareto principle [^2] to include the 20% most important 
tests in the daily process in order to always try to catch 80% of the errors.

[^2]: Pareto principle – http://en.wikipedia.org/wiki/Pareto_principle

## Communication Everyone should easily know the state and the changes made to the system.

A common repository, plus daily submits, plus a Continuous Integration server is also helpful as a communication tool. 
Everybody in the team will feel better and get updated about everybody's work.

## Production. Make the integration and tests in a production clone

It is highly desirable to try clone every aspect of production in our CI Server. This is necessary in order to avoid 
false positives or negatives when we execute our tests.

# Benefits

Some benefits of implementing this practice:

* Reduced risks.
* No debug. As you can always return to a known bug free state.
* Fact. Projects that use CI techniques usually have fewer bugs.
* You will be always developing on a known stable base.
* Solution to the Broken Window syndrome.
* Frequent deployments.
* Early warnings.
* Integration is a natural continuous process.
* Constant availability of a “current” build for testing, demo, or release purposes.
* Incentive for developers to code incrementally.
* Quality. Due to metrics generated from automated testing and CI (such as code coverage, code complexity, etc)


# References

- Martin Fowler on Continuous Integration – http://www.martinfowler.com/articles/continuousIntegration.html
- About Continuous Integration - http://en.wikipedia.org/wiki/Continuous_Integration
- About Agile and XP – http://agilemanifesto.org/principles.html
- CI servers matrix – http://confluence.public.thoughtworks.org/display/CC/CI+Feature+Matrix
- Grady Booch, Desgin Strategies – http://www.amazon.com/Best-Booch-Designing-Strategies-Technology/dp/0137396163/ref=sr_1_2?ie=UTF8&s=books&qid=1262278301&sr=8-2

