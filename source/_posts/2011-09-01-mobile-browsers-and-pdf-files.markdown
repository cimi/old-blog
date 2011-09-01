---
layout: post
title: "Mobile Browsers and PDF Files"
date: 2011-09-01 00:39
comments: true
categories: [mobile, web, HTTP]
---

Troubleshooting a PDF download tool, I was surprised to find out that some mobile browsers behave unexpectedly when trying to directly access a PDF resource through HTTP. While it was pretty obvious that they are lightweight and do not have PDF viewing plugins embedded as their desktop counterparts, even downloading the resource was behaving strangely in some of them.

I tested on my Samsung Galaxy S (GTi9000), a Palm Pre Plus, a Blackberry Torch (9800) and an iPhone 4. All browsers are WebKit based. The test case was pretty simple - download a PDF file from my domain. The PDF was ~200KB in size, to keep the tests quick. 

To debug the actual HTTP traffic of the devices, I interposed Fiddler as a reverse proxy between my domain and them. This way, I was able to log all the headers and content of all the HTTP requests and responses made.

Let's start with the responses, the server was not configured in any way to serve PDFs, so it gave the same response regardless of the request headers it received.

```
    HTTP/1.1 200 OK
    Date: Tue, 02 Aug 2011 09:18:05 GMT
    Accept-Ranges: bytes
    ETag: "2f257-4e36a6c5-0"
    Last-Modified: Mon, 01 Aug 2011 13:14:45 GMT
    Content-Type: application/pdf
    Content-Length: 193111
    Connection: Keep-Alive
    Age: 13251
```

The caching headers are automatically inserted by the server, but they did not seem to affect the devices since the content was being downloaded for every request. 

Now for the interesting part: besides the iPhone browser, all others make *two requests* for the file, which means the content is downloaded twice!

The BlackBerry first sends a complete request and then repeats it identically: 

```
    GET /a.pdf HTTP/1.1
    Host: improve.ro
    Connection: keep-alive
    User-Agent: Mozilla/5.0 (BlackBerry; U; BlackBerry 9800; en-GB) AppleWebKit/534.1+ (KHTML, like Gecko) Version/6.0.0.246 Mobile Safari/534.1+
    Accept: text/html,application/xhtml+xml,application/xml,*/*;q=0.5
    Accept-Language: en-GB,en;q=0.5
    Accept-Encoding: gzip,deflate
    x-wap-profile: "http://www.blackberry.net/go/mobile/profiles/uaprof/9800_80211g/6.0.0.rdf"
```

```
    GET /a.pdf HTTP/1.1
    Host: improve.ro
    Connection: keep-alive
    User-Agent: Mozilla/5.0 (BlackBerry; U; BlackBerry 9800; en-GB) AppleWebKit/534.1+ (KHTML, like Gecko) Version/6.0.0.246 Mobile Safari/534.1+
    Accept: text/html,application/xhtml+xml,application/xml,*/*;q=0.5
    Accept-Language: en-GB,en;q=0.5
    Accept-Encoding: gzip,deflate
    x-wap-profile: "http://www.blackberry.net/go/mobile/profiles/uaprof/9800_80211g/6.0.0.rdf"
```

The Samsung Galaxy S browser sends a complete request, then a stripped down version of it:

```
    GET /a.pdf HTTP/1.1
    Host: improve.ro
    Connection: keep-alive
    Accept-Encoding: gzip
    Accept-Language: en-US
    x-wap-profile: http://wap.samsungmobile.com/uaprof/GT-i9000.xml
    User-Agent: Mozilla/5.0 (Linux; U; Android 2.2.1; en-us; GT-I9000 Build/FROYO) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1
    Cookie: ASP.NET_SessionId=p2k2g4rv1f3eily0gifmimwf
    Accept: application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q=0.8,image/png,*/*;q=0.5
    Accept-Charset: utf-8, iso-8859-1, utf-16, *;q=0.7
```

```
    GET /a.pdf HTTP/1.1
    Cookie: ASP.NET_SessionId=p2k2g4rv1f3eily0gifmimwf
    Host: improve.ro
    Connection: Keep-Alive
    User-Agent: Mozilla/5.0 (Linux; U; Android 2.2.1; en-us; GT-I9000 Build/FROYO) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1
```

The Palm Pre Plus behaves similarly with the Galaxy, only it doesn't even send a `User-Agent`:

```
    GET /a.pdf HTTP/1.1
    Host: improve.ro
    Accept-Encoding: gzip, deflate
    User-Agent: Mozilla/5.0 (webOS/1.4.2; U; en-US) AppleWebKit/532.2 (KHTML, like Gecko) Version/1.0 Safari/532.2 Pre/1.1
    Accept: application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q=0.8,image/png,*/*;q=0.5
    Accept-Language: en-us,en;q=0.5
    X-Palm-Carrier: c001-01
    accept-charset: ISO-8859-1,utf-8;q=0.7,*;q=0.3
```

```
    GET /a.pdf HTTP/1.1
    Host: improve.ro
    Accept: */*
```

This is the only request the iPhone made.

```
    GET /hp/a.pdf HTTP/1.1
    Host: improve.ro
    User-Agent: Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_3_4 like Mac OS X; en-us) AppleWebKit/533.17.9 (KHTML, like Gecko) Version/5.0.2 Mobile/8K2 Safari/6533.18.5
    Accept: application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q=0.8,image/png,*/*;q=0.5
    Accept-Language: en-us
    Accept-Encoding: gzip, deflate
    Connection: keep-alive
```

Weird!
