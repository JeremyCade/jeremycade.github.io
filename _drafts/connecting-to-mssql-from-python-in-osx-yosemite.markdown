---
layout: post
title:  "Connecting to Microsoft SQL Server with python in OS X Yosemite"
categories: python sql-server osx yosemite
---

Thanks in part to my technology ADD, and desire to be a general language polyglot; it is by no means unusual to have a mixed technology stack.

In this case I have a number of python micro-services and command line applications, running on a Mac Mini loaded with OS X Yosemite, that need to communicate with Microsoft's SQL Sever 2008 R2, on top of Windows Server 2008 R2.

While SQL Server is a great Relational Database Management System (RDBMS), it isn't exactly known for it's ease of use when it comes to cross-platform communication. A quick Google Search will return quite a range of results (and horror stories) of 

## Getting Started
It is assumed that you already have a working Python installation, either in the Python 2.7+ or Python 3.3+ families and [Pip](https://pypi.python.org/pypi/pip). 

Additionally, we will need: 

- [Homebrew](http://brew.sh/) or [Macports](https://www.macports.org/)
- [FreeTDS](http://www.freetds.org/)
- [pymssql](http://pymssql.org/en/latest/) or [pyodbc](https://code.google.com/p/pyodbc/)

### Installing Homebrew or Macports
I personally prefer to use [Homebrew](http://brew.sh/) due to it's simplicity; though I do have a MacBook with [Macports](https://www.macports.org/) installed.

### Installing FreeTDS
[FreeTDS](http://www.freetds.org/) is a set of *nix libraries, that allow applications to talk to Microsoft SQL Server.

In your typical *nix environment FreeTDS sources configurations from two difference places:

    /etc/freetds.conf # global
    ~/.freetds.conf # user / local

This is slightly different under OS X, and will vary depending on the package manager used to install FreeTDS.

#### Homebrew
{% highlight bash %}
brew install freetds
{% endhighlight %}

Homebrew symlinks the global *freetds.conf* file to:

    /usr/local/etc/freetds.conf

#### Macports
{% highlight bash %}
sudo port install freetds
{% endhighlight %}

Macports installs the global *freetds.conf* file to:

    /opt/local/etc/freetds/freetds.conf

### Testing FreeTDS
{% highlight bash %}
tsql -H <host> -p <port> -U <username> -P <password>
{% endhighlight %}
If everything is working you will see something along the lines of:

{% highlight bash %}
locale is "en_AU.UTF-8"
locale charset is "UTF-8"
using default charset "UTF-8"
{% endhighlight %}

My Macports machine presented the following errors:

{% highlight bash %}
locale is "en_AU.UTF-8"
locale charset is "UTF-8"
using default charset "UTF-8"
Error 20018 (severity 9):
    Unexpected EOF from the server
    OS error 36, "Operation now in progress"
Error 20002 (severity 9):
    Adaptive Server connection failed
There was a problem connecting to the server.
{% endhighlight %}

This was overcome by specifying the TDS Version as an Environment Variable like so:

{% highlight bash %}
TDSVER=7.0 tsql -H <host> -p <1433 or custom port> -U <username> -P <password>
{% endhighlight %}

