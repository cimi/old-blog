--- 
wordpress_id: 13
layout: post
title: Proxy settings with HTTPClient and Spring
date: Sat Oct 30 21:16:06 +0300 2010
categories: 
  slug: java
  title: java
  autoslug: java
wordpress_url: http://improve.ro/?p=13
---
Since I didn't find any example on the Web on how to do this, I'll give it a go here and try to explain things a bit. I'm going to treat the particular case of using [HTTPClient](http://hc.apache.org/httpcomponents-client-ga/index.html) and [Spring](http://www.springsource.org/). For changing the proxy value in the JRE, see the official documentation on [Java Networking and Proxies](http://java.sun.com/javase/6/docs/technotes/guides/net/proxies.html).

From the [documentation](http://hc.apache.org/httpcomponents-client-ga/tutorial/html/connmgmt.html#d4e540), we see that in order to set up a proxy for requests made through HTTPClient you have three options: specifying it directly, getting it from the JRE or implementing a custom RoutePlanner to have complete control over the HTTP route computation. 

To specify it explicitly, you need to change a parameter in the HTTPClient configuration: 

``` java
    DefaultHttpClient httpclient = new DefaultHttpClient();
    HttpHost proxy = new HttpHost("someproxy", 8080);
    httpclient.getParams().setParameter(ConnRoutePNames.DEFAULT_PROXY, proxy);
```

We need to have an HttpHost object that contains the proxy information. To take advantage of Spring's dependency injection, you can rely on the configuration files to create that object. You can define a proxy bean like this:

``` xml    
    <bean id="proxy" class="org.apache.http.HttpHost">
        <constructor-arg value="[host-name]" />
        <constructor-arg value="[port]" />
    </bean>
``` 

And then, use dependency injection to link it to the classes that need it:

``` xml
    <bean id="yahooGeocodingService" class="YahooGeocodingService">
        <constructor-arg ref="proxy" />
        <!-- [...] -->
    </bean>
```

``` java
    public class YahooGeocodingService {    
        // [...]    
        public YahooGeocodingService(HttpHost proxy, ...) {        
            this.proxy = proxy;    
        }    
        // [...]
    }
```

If the proxy bean definition is missing, _null_ will be passed as an argument to the constructor. By creating a wrapper for HTTP requests we can selectively apply the proxy, based on if it was set or not:

``` java 
    private static HttpClient enableProxy(HttpClient httpclient, HttpHost proxy, URI target) {
        // if the request is for localhost or no proxy host was passed do not set it
        if ((target.getHost().equalsIgnoreCase("localhost")) || (proxy == null))
            return httpclient;
        // set the proxy
        httpclient.getParams().setParameter(ConnRoutePNames.DEFAULT_PROXY, proxy);
        return httpclient;
    }
```

So now, in the method where you actually want to do the request, you just have to add one line if you want proxy support for the HttpClient object you've created: 

``` java
    Utils.enableProxy(httpClient, this.proxy, target)
```

