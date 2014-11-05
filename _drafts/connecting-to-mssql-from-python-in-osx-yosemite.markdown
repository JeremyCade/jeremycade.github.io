---
layout: post
title:  “Connecting to SQL Server from python in OS X Yosemite”
date:   2014-11–05
categories: python sql-server osx yosemite
---

[pyodbc](https://code.google.com/p/pyodbc/)

### Installing
#### Homebrew
    brew install freetds

The homebrew installation seems to work fine without the optional [unixodbc](http://www.unixodbc.org/) dependency.

#### Macports
    sudo port install freetds +odbc +mssql



### Testing freetds
    tsql -H <host> -p <1433 or custom port> -U <username> -P <password>

If everything is working you will see something along the lines of:
    locale is "en_AU.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"

