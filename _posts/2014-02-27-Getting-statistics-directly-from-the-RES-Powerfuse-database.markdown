---
layout: post
title: Getting statistics directly from the RES Powerfuse database
date: 2014-02-27 13:37:00 +0100
description: Getting statistics directly from the RES Powerfuse database
img: header/getting-statistics-directly-from-the-res-powerfuse-database.png
tags: [PowerShell, PoSh, Scripting, Windows, RES, PowerFuse, Database, SQL, MSSQL]
---
Directly gathering information from RES Powerfuse using a MS-SQL query is, compared to the cumbersome PowerFuse Usage Tracker Viewer, easy to do.

To generate a list of all applications, sorted on the application name, you can use the following query.

    select b.strtitle, count(1)
    from
    (select distinct a.struser, a.strtitle
    from dbo.TBLhistory a
    where a.lngappid > 1) b
    group by b.strtitle
    order by b.strtitle;

Example of the result:

    7-Zip 43
    Access 57
    Access Management Console 24
    ACDSee 19
    Acrobat Reader 131
    Active Directory Users and Computers 12
    Adobe Acrobat Pro 1
    Adobe Reader 846

If you would like to know which application has the highest usage count, a little adaptation to the query is needed.

    select b.strtitle, count(1)
    from
    (select distinct a.struser, a.strtitle
    from dbo.TBLhistory a
    where a.lngappid > 1) b
    group by b.strtitle
    order by COUNT(1) desc;

Example of the result:

    Adobe Reader 846
    Acrobat Reader 131
    Access 57
    7-Zip 43
    Access Management Console 24
    ACDSee 19
    Active Directory Users and Computers 12
    Adobe Acrobat Pro 1
