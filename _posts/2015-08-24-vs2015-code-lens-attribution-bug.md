---
layout: post
title:  "Visaul Studio 2015: Code Lens Unit Test Attribution bug"
date:   2015-08-24
comments: true
categories: visual-studio-2015
---
I am currently working on a legacy application that has recently had a "solution" upgrade to work with Visual Studio 2015.
During the course of my work I have encountered minor Code Lens attribution bug. 

The bug essentially attributes test results of a child class, to the base class from which it is inherited. 
The test results are then propagated up the class hierarchy to all classes which inherit from the base class. 

### Example:

1.  Create Unit Tests for new Attribute 
![Figure: Unit Tests for RemoteRequireAttribute]({{ site.url }}/images/VS2015/CodeLens/01.png)
**Figure: Unit Tests for RemoteRequireAttribute**

2.  Create ActionFilter that inherits from RequireHttpsAttribute | ActionFilterAttribute and implement functionality.
![Figure: RemoteRequireHttpsAttribute class signature]({{ site.url }}/images/VS2015/CodeLens/02.png)
**Figure: RemoteRequireHttpsAttribute class signature**

3.  Execute Tests. All Passing
![Figure: Passing Unit Tests]({{ site.url }}/images/VS2015/CodeLens/03.png)
**Figure: Passing Unit Tests**

4.  View another class that inherits from ActionAttribute. VS2015 Code lens now shows 5 of 5 tests passing.
![Figure: 5 of 5 passing tests]({{ site.url }}/images/VS2015/CodeLens/04.png)
**Figure: 5 of 5 passing tests**

5.  Closer inspection of the tests shows that the are not related to this class, other than the base inherited classes.
![Figure: 5 of 5 passing tests are all for RemoteRequireHttpsAttribute ActionFilter.]({{ site.url }}/images/VS2015/CodeLens/05.png)
**Figure: 5 of 5 passing tests are all for RemoteRequireHttpsAttribute ActionFilter.**


For those of you who are wondering what my system looks like:

- Surface Pro 3 - Max specs. 
- Windows 8.1 Fully patched and at the latest version
- Domain Joined
- .Net 2 through 4.6 installed and fully patched to the latest versions
- VS2013 and VS2015 are both installed and fully patched. 
