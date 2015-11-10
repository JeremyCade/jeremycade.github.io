---
layout: post
title:  "Connecting to Microsoft SQL Server with python in OS X Yosemite"
date:   2014-11-12
comments: true
categories: python sql-server osx yosemite
---

Due in part to my technology ADD, and desire to be a polyglot developer; it is by no means unusual to find myself in a position where I am working with a mixed technology stack.

In this case I have a number of python micro-services and command line applications running on a Mac Mini loaded with OS X Yosemite that need to retrieve data from a Microsoft SQL Sever 2008 R2 instance on Windows Server 2008 R2.

While SQL Server is a great Relational Database Management System (RDBMS), it isn't exactly known for it's ease of use when it comes to cross-platform communication. A quick Google Search will return a range of results (and horror stories) on how to get this all to work. 

## Getting Started
It is assumed that you already have a working Python installation (Python 2.7+ or 3.3+) and [Pip](https://pypi.python.org/pypi/pip). 

Additionally, we will need: 

- [Homebrew](http://brew.sh/) or [Macports](https://www.macports.org/)
- [FreeTDS](http://www.freetds.org/)
- [pymssql](http://pymssql.org/en/latest/)

#### Decisions and Rationale
I personally prefer to use [Homebrew](http://brew.sh/) due to it's simplicity; though I do have a MacBook with [Macports](https://www.macports.org/) installed.

The [pymssql](http://pymssql.org/en/latest/) vs [pyodbc](https://code.google.com/p/pyodbc/) argument is not one I really care to worry about. Though I generally choose to use pymssql due to better support for MS SQL Stored Procedures, and a less troublesome install. I have had issues in the past installing pyodbc via Pip.

### Installing FreeTDS
[FreeTDS](http://www.freetds.org/) is a set of *nix libraries, that allow applications to talk to Microsoft SQL Server.

In your typical *nix environment FreeTDS sources configuration from two difference places:

    ${HOME}/.freetds.conf
    /etc/freetds.conf

This is slightly different under OS X, and will vary depending on the package manager used to install FreeTDS. 

#### Homebrew Install

    brew install freetds

Homebrew symlinks the global *freetds.conf* file to:

    /usr/local/etc/freetds.conf

#### Macports Install

    sudo port install freetds

Macports installs the global *freetds.conf* file to:

    /opt/local/etc/freetds/freetds.conf

### FreeTDS Configuration

FreeTDS reads the `${HOME}/.freetds.conf` before resorting to the global `freetds.conf`. You can find out what the location of your global `freetds.conf` by executing the following command:

    tsql -C

Your result will be something along these lines:

    $ tsql -C
    Compile-time settings (established with the "configure" script)
                                Version: freetds v0.91
                 freetds.conf directory: /usr/local/Cellar/freetds/0.91_2/etc
         MS db-lib source compatibility: no
            Sybase binary compatibility: no
                          Thread safety: yes
                          iconv library: yes
                            TDS version: 7.1
                                  iODBC: no
                               unixodbc: no
                  SSPI "trusted" logins: no
                               Kerberos: no


Full details about the FreeTDS configuration files can be found [here](http://www.freetds.org/userguide/freetdsconf.htm).

### Testing FreeTDS

    tsql -H <host> -p <port> -U <username> -P <password>

If everything is working you will see something along the lines of:

    locale is "en_AU.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"

My Macports machine presented the following errors:

    locale is "en_AU.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Error 20018 (severity 9):
        Unexpected EOF from the server
        OS error 36, "Operation now in progress"
    Error 20002 (severity 9):
        Adaptive Server connection failed
    There was a problem connecting to the server.

This was overcome by specifying the TDS Version as an Environment Variable like so:

    TDSVER=7.0 tsql -H <host> -p <1433 or custom port> -U <username> -P <password>

Your other option for overcoming this type of error is to specify DNS names within your local `freetds.conf` file, for example:

    [myhost]
    	host = dbdev.mydomain.com
    	port = 1433
    	tds version = 7.0

You can this test this by using the following command:

    tsql -S myhost -U <USER> -P <PASSWORD>

### Installing pymssql
pymssql is easily installed on your machine via Pip. 

    pip install pymssql

Once installed you can test the package in a fairly simple manner via Python interactive. 

{% highlight python %}
$ python3 -i
Python 3.4.2 (default, Oct 17 2014, 20:25:14) 
[GCC 4.2.1 Compatible Apple LLVM 6.0 (clang-600.0.51)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import pymssql
>>> conn = pymssql.connect("HOST","USER","PASSWORD","DATABASE")
>>> cursor = conn.cursor()
>>> cursor.execute("select * from dbo.[TABLE]")
>>> for row in cursor:
...     print("row = %r" % (row, ))
... 
row = (1, 'LITERAL', Decimal('123.123'))
row = (2, 'LITERAL', Decimal('123.123'))
row = (3, 'LITERAL', Decimal('123.123'))
row = (4, 'LITERAL', Decimal('123.123'))
row = (5, 'LITERAL', Decimal('123.123'))
row = (6, 'LITERAL', Decimal('123.123'))
>>> conn.close()
>>> exit()
{% endhighlight %}

pymssql adheres to the [Python DB-API 2.0](http://legacy.python.org/dev/peps/pep-0249/) specification. Meaning that all of the standard cursor methods are available. It also allows you to use ORM packages like [SQLAlchemy](http://docs.sqlalchemy.org/en/rel_0_9/dialects/mssql.html#module-sqlalchemy.dialects.mssql.pymssql). pymssql also allows you to call [MS SQL Stored procedures](http://pymssql.org/en/latest/pymssql_examples.html#calling-stored-procedures) via the *cursor.execproc* method, which for me is a must have feature, particularly when dealing with odd inflexible (EVERYTHING MUST BE A SP) DBA. 
