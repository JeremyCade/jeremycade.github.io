---
layout: post
title:  "Generating Page Slugs in T-SQL!"
date:   2013-12-10
comments: true
categories: t-sql page-slugs
---

I recently came across a situation where I needed to generate page Slugs from some 5,000,000+ records in a MS-SQL database. Unfortunately I didn’t have Remote Desktop or Telnet access to the SQL box which precluded me from running one of my normal C# or Powershell solutions, and the idea of doing all of the processing over the wire just didn’t appeal to me. So I decided the best course of action would be to use T-SQL to generate the Slugs for me.

After posting the question to [StackOverflow](http://stackoverflow.com/questions/3082588/t-sql-function-for-generating-slugs) and doing some searching on Google I came across Pinal Dave’s [UDF_ParseAlphaChars](http://blog.sqlauthority.com/2007/05/13/sql-server-udf-function-to-parse-alphanumeric-characters-from-string/) function. Luckily it had most of the functionality I required, so it only required minor modifications to get the desired result.

The modified code is below.

{%highlight sql linenos %}
    CREATE FUNCTION [dbo].[UDF_GenerateSlug]
    (
        @str VARCHAR(100)
    )
    RETURNS VARCHAR(100)
    AS
    BEGIN	
        DECLARE @IncCharLoc SMALLINT
        SET @str = LOWER(@str)
        SET @IncCharLoc = PATINDEX('%[^0-9a-z] %',@str)
        
        WHILE @IncCharLoc &gt; 0
        BEGIN
        	SET @str = STUFF(@str,@IncCharLoc,1,'')
        	SET @IncCharLoc = PATINDEX('%[^0-9a-z] %',@str)
        END
        
        SET @str = REPLACE(@str,' ','-')
        RETURN @str
        END 
    GO
{% endhighlight %}

First of all I need to leave spaces so they can be replaced with hyphens, and secondly I only require lower case characters. You may also notice that I'm doing a case transformation against the incoming string. This is due to the SQL database being setup with case insensitivity.

I would like to thank Pinal Dave for his original code.