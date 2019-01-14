---
layout: post
title: How to launch and watch a job on Pentaho/Carte Server 7.x+
categories: [shell]
tags: [pentaho,jenkins,bash]
fullview: false
comments: true
---

Since version 7.x, Pentaho Server (or a Carte Server) has the ability to kick off jobs (or transformations) on one of the aforementioned servers. 

To see the status of the ETL that is being executed, one would browse to `ht<span>tp</span>://SERVER_IP:SERVER_PORT/kettle/status`.  We could actually see this status page of currently and recently executed ETL since at least version 4.x.

The shell script provided starts execution of a job stored in the Pentaho Repository (**NOTE: syntax may change slightly between versions, and Enterprise or File System repositories)** and monitors the status of the job on the kettle/status endpoint by calling it with the id generated after submission.  

The status of the job is then evaluated to identify overall SUCCESS or FAILURE state of the requested job. This can be used with scheduling tools or CI/CD tools, for regular execution, or perhaps in a testing framework, respectively. 

<a href="https://github.com/kiranrajendran/krghio/blob/master/j_run_something_main.sh" target="_blank">Click here for script</a>\
<a href="http://help.pentaho.com" target="_blank">Click here for Pentaho Documentation</a>