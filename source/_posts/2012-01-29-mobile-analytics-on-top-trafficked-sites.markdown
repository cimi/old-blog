---
layout: post
title: "Mobile Analytics on Top Trafficked Sites"
date: 2012-01-29 23:16
comments: true
categories: [mobile, JavaScript, web] 
---

This is a short analysis on how web analytics are implemented across a few top trafficked websites. Part of them were picked from the [Alexa rakings for shopping websites](http://www.alexa.com/topsites/category/Top/Shopping) and some are the homes of popular tech giants.

I tested this in Mozilla Firefox with a spoofed iPhone 3 user-agent (via the [User-Agent Switcher](https://addons.mozilla.org/en-US/firefox/addon/user-agent-switcher/) plugin). The drawback of this method is that for sites using feature detection and client-side redirection versus redirection based on the UA string I would still be getting the desktop site. That being said, I didn't find any of the sites to have this behavior.

Also, I didn't check all of them with a featured phone UA. I only did this for sites that had clues to different enhanced/featured implementations in their URL structure. The only one that had this, from the sites I tested was Groupon.

I used [Firebug](http://getfirebug.com) to check the requests being made and the size of the scripts. I also used the online JS Beautifier and the closure-compiler interface to swing back and forth between minified and unminified scripts. 

I tried the domain of the companies directly (i.e., canon.com) and I did not navigate deeper into the site structure for any of the tested websites. It may be possible that some sites who reported to not have a mobile optimized site to simply not redirect to it when accessing them directly.

<!-- more -->

[Amazon](http://www.amazon.com/gp/aw/h.html/190-2635517-0125555 "Amazon Mobile") uses inline JavaScript to build the URL of an analytics beacon. The code is minified and the total size (including script, noscript, img and a div tag) is 5KB. This comes to about 2KB gzipped.

[eBay](http://hp.mobileweb.ebay.com/home "eBay Mobile") and [Walmart](http://mobile.walmart.com/ "Walmart Mobile") do not seem to have any form of analytics code sent on the client side. Probably some form of tracking is done on the server, it being transparent to the client.

[BestBuy](http://m.bestbuy.com/m/e/ "BestBuy Mobile") directly embeds an Omniture beacon in the page. There is no extra JavaScript to download and execute. It also has an embedded Atlas beacon image.

Groupon has separate sites for [enhanced](http://touch.groupon.com/ "Groupon for smart phones") and [featured](http://m.groupon.com "Groupon for dumb phones") devices. They both use [rum.js](https://d1ros97qkrwjf5.cloudfront.net/30/eum/rum.js), a 3KB script that generates a beacon URL for Google Analytics.

[IKEA](http://m.ikea.com/ "IKEA Mobile") has its analytics code bundled inside [it's 50KB (zipped) JavaScript file](http://m.ikea.com/irmw-resources/js/ikea.mobile.min.js "IKEA mobile script"). The size of the analytics code is 67KB minified which becomes 22KB gzipped.

[Target](http://sites.target.com/site/en/spot/mobile.jsp "Target Mobile") uses Omniture for tracking and includes [a 47KB (17KB gzipped) JavaScript](http://sites.target.com/js/mobile_omniture.js) in the page. An interesting fact is that their analytics script is more than twice as large [as their JavaScript](http://sites.target.com/js/mobile_main.js "Target Mobile Javascript"), which is only 7.5KB zipped. 

[Newegg](http://www.newegg.com/ "newegg mobile") uses [IBM Coremetrics](http://coremetrics.com/) and embeds a beacon image directly in the page.

[Dell](http://m.dell.com/mt/www.dell.com "Dell Mobile") also uses Omniture and downloads a 50KB (19KB gzipped) JavaScript.

[IBM](http://m.ibm.com/us/en/ "IBM Mobile") sends a beacon to unica.com. The beacon URL is built by a [downloaded JavaScript file](http://www.ibm.com/common/stats/stats.js "IBM Metrics Script"). The file size is 18.6KB minified (6.8KB gzipped).

[Microsoft](http://m.microsoft.com "Microsoft Mobile") uses a directly embedded beacon image that hits [webtrends.com](http://webtrends.com/ "webtrends").

Apple, SAP, Canon, Asus, Toshiba, Fujitsu and Acer do not serve optimized content for mobile - or at least I was not able to find their mobile site given the methods described in the beginning.

<script type="text/javascript" src="http://www.google.com/jsapi"></script>
<script type="text/javascript">
  google.load('visualization', '1', {packages: ['corechart']});
</script>

<script type="text/javascript">
  function drawVisualization() {
    // Create and populate the data table.
    var data = new google.visualization.DataTable();
    var raw_data = [
      ['IKEA', 67, 22],
      ['HP', 53, 20],
      ['Dell', 50, 19],
      ['Target', 47, 17],
      ['IBM', 18.6, 6.8],
      ['Groupon', 9, 3],
      ['Amazon', 5, 2],
      ['eBay', 0, 0],
      ['Walmart', 0, 0],
      ['Newegg', 0, 0],
      ['Microsoft', 0, 0]
    ];
    
    var types = ['Uncompressed', 'Compressed'];
                    
    data.addColumn('string', 'Year');
    for (var i = 0; i  < raw_data.length; ++i) {
      data.addColumn('number', raw_data[i][0]);    
    }
    
    data.addRows(types.length);
  
    for (var j = 0; j < types.length; ++j) {    
      data.setValue(j, 0, types[j].toString());    
    }
    for (var i = 0; i  < raw_data.length; ++i) {
      for (var j = 1; j  < raw_data[i].length; ++j) {
        data.setValue(j-1, i+1, raw_data[i][j]);    
      }
    }
    
    // Create and draw the visualization.
    new google.visualization.ColumnChart(document.getElementById('visualization')).
        draw(data,
             {
               title:"Size of Mobile Metrics Code", 
               width: 800, height: 400,
               vAxis: {
                 format: '#.##KB',
                 baseline: -2,
                 minValue: 0,
                 viewWindow: { min: -2 }
               }
             }
        );
  }
  

  google.setOnLoadCallback(drawVisualization);
</script>

<div id="visualization" style="width: 800px; height: 400px;"></div>

I posted the code used to generate the chart in a [gist](https://gist.github.com/1704731). Feel free to update/correct it.
