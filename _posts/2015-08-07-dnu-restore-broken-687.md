---
layout: post
title:  "ASP.NET 5 Beta 6: dnu restore broken #687"
date:   2015-08-07
comments: true
categories: asp.net
---

I have recently updated to the latest ASP.NET 5 Beta on my OS X development machines. One of the first stumbling blocks that I ran into was issue [#687: dnu restore broken](https://github.com/aspnet/Home/issues/687).

![dnu execution error.]({{ site.url }}/images/dnu_execution_error.png)
**Figure: Permission is denied.**

This is a minor issue, and the fix is relatively simple

**Steps to fix:**

1. Open Terminal `⌘+space Terminal`
2. Execute the following: 

````bash    
    cd ~/.dnx/runtimes/dnx-mono.1.0.0-beta6/bin/
    chmod +x dnu
````
No more issues.

![Correct permissions for dnu execution.]({{ site.url }}/images/dnu_executeable.png)
**Figure: Executeable dnu**