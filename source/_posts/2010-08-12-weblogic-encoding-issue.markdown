--- 
wordpress_id: 44
layout: post
title: Weblogic Encoding Issue
date: Thu Aug 12 14:28:43 +0300 2010
categories: 
  slug: troubleshooting
  title: troubleshooting
  autoslug: troubleshooting
wordpress_url: http://improve.ro/?p=44
---
Trying to deploy some RSS feeds as .jspx views on a [WebLogic 10 server](http://en.wikipedia.org/wiki/Oracle_WebLogic_Server "Oracle WebLogic"), I noticed that it mangled all UTF-8 output. This was part of a Spring MVC web-application. The problem was that on my local development server (Apache Tomcat 6.0) everything rendered fine, but on the WebLogic server all non-ANSI characters were not outputted correctly.

In Firefox, I saw something like: `<summary>Formaci�n</summary>`. The byte sequence for the strange character was `0xEF 0xBF 0xBD` and I seemed to get that for all UTF-8 chars that I was supposed to receive in the tests I was conducting (á, ó, í). I checked the content-type and encoding in Firebug and it seemed ok (`Content-Type: application/xhtml+xml; charset=UTF-8`).I later found out that `�` is the [Unicode Replacement Character U+FFFD](http://www.fileformat.info/info/unicode/char/fffd/index.htm) and that the problem was probably caused by the fact that the server, although told to output UTF-8, sent out ISO-8859-1. 

The fix came from my .jspx files, more specifically the page directive tag. What surprised me was the fact that the **order of attributes** in the .jspx page directive matters! Initially I had this:

``` xml
    <jsp:directive.page pageEncoding="utf-8" contentType="application/xhtml+xml" />
```

This doesn't work, because you also need to specify the charset in the contentType attribute:

``` xml
    <jsp:directive.page contentType="application/xhtml+xml; charset=UTF-8" pageEncoding="UTF-8" />
```

The above line works and determines the correct encoding. But, to my surprise, if you switch the order of attributes **it doesn't work:**

``` xml
    <jsp:directive.page pageEncoding="UTF-8" contentType="application/xhtml+xml; charset=UTF-8" />
```

I don't know where this is coming from, but, in my opinion it's a bug in WebLogic. I'll open a bug-report.You can also find details about this issue on my [Stackoverflow post](http://stackoverflow.com/questions/3317711/encoding-errors-in-jspx "StackOverflow: Encoding errors in .jspx"). Thanks to [BalusC](http://balusc.blogspot.com/ "The BalusC Code") for the help.
