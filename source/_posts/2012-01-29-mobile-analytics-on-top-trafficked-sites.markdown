---
layout: post
title: "Mobile Analytics on Top Trafficked Sites"
date: 2012-01-29 23:16
comments: true
categories: 
---

This is a short analysis on how web analytics are implemented across a few top trafficked websites. Part of them were picked from the Alexa rakings for shopping websites and some are the homes of popular tech giants.

I tested this in Mozilla Firefox with a spoofed iPhone 3 user-agent (via the User-Agent Switcher plugin). The drawback of this method is that for sites using feature detection and client-side redirection versus redirection based on the UA string I would still be getting the desktop site. That being said, I didn't find any of the sites to have this behavior.

Also, I didn't check all of them with a featured phone UA. I only did this for sites that had clues 

I used Firebug to check the requests being made and the size of the scripts. I also used the online JS Beautifier and the closure-compiler interface to swing back and forth between minified and unminified scripts. 

I tried the domain of the companies directly (i.e., canon.com) and I did not navigate deeper into the site structure for any of the tested websites. It may be possible that some sites who reported to not have a mobile optimized site to simply not redirect to it when accessing them directly.

<table>
	<thead>
		<tr>
			<td>Company</td>
			<td>Analytics Provider</td>
			<td>Size of JavaScript</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Amazon</td>
			<td>unknown</td>
			<td>5KB (2KB)</td>
		</tr>
		<tr>
			<td>eBay</td>
			<td>unknown</td>
			<td>0KB</td>
		</tr>
		<tr>
			<td>Walmart</td>
			<td>unknown</td>
			<td>0KB</td>
		</tr>
		<tr>
			<td>BestBuy</td>
			<td>Omniture</td>
			<td>0KB</td>
		</tr>
		<tr>
			<td>Groupon</td>
			<td>Google Analytics</td>
			<td>13KB / 3KB</td>
		</tr>
		<tr>
			<td>IKEA</td>
			<td>Omniture</td>
			<td>67KB (22KB)</td>
		</tr>
		<tr>
			<td>Target</td>
			<td>Omniture</td>
			<td>47KB (17KB)</td>
		</tr>
		<tr>
			<td>Newegg</td>
			<td>IBM Coremetrics</td>
			<td>0KB</td>
		</tr>
		<tr>
			<td>IBM</td>
			<td>IBM Unica</td>
			<td>18.6KB (6.8KB)</td>
		</tr>
		<tr>
			<td>Microsoft</td>
			<td>Webtrends</td>
			<td>0KB</td>
		</tr>
		<tr>
			<td>Dell</td>
			<td>Omniture</td>
			<td>50KB (19KB)</td>
		</tr>
	</tbody>
</table>

[Amazon](http://www.amazon.com/gp/aw/h.html/190-2635517-0125555 "Amazon Mobile") uses inline JavaScript to build the URL of an analytics beacon. The code is minified and the total size (including script, noscript, img and a div tag) is 5KB. This comes to about 2KB gzipped.

[eBay](http://hp.mobileweb.ebay.com/home "eBay Mobile") and [Walmart](http://mobile.walmart.com/ "Walmart Mobile") do not seem to have any form of analytics code sent on the client side. Probably some form of tracking is done on the server, it being transparent to the client.

[BestBuy](http://m.bestbuy.com/m/e/ "BestBuy Mobile") directly embeds an Omniture beacon in the page. There is no extra JavaScript to download and execute. It also has an embedded Atlas beacon image.

Groupon has separate sites for [enhanced](http://touch.groupon.com/ "Groupon for smart phones") and [featured](http://m.groupon.com "Groupon for dumb phones") devices. The one for enhanced uses the 13KB Google Analytics script, the one for featured devices uses rum.js, a 3KB script that generates the beacon URL for Google Analytics.

[IKEA](http://m.ikea.com/ "IKEA Mobile") has its analytics code bundled inside [it's 50KB (zipped) JavaScript file](http://m.ikea.com/irmw-resources/js/ikea.mobile.min.js "IKEA mobile script"). The size of the analytics code is 67KB minified which becomes 22KB gzipped.

[Target](http://sites.target.com/site/en/spot/mobile.jsp "Target Mobile") uses Omniture for tracking and includes [a 47KB (17KB gzipped) JavaScript](http://sites.target.com/js/mobile_omniture.js) in the page. An interesting fact is that their analytics script is more than twice as large [as their JavaScript](http://sites.target.com/js/mobile_main.js "Target Mobile Javascript"), which is only 7.5KB zipped. 

[Newegg](http://www.newegg.com/ "newegg mobile") uses [IBM Coremetrics](http://coremetrics.com/) and embeds a beacon image directly in the page.

[Dell](http://m.dell.com/mt/www.dell.com "Dell Mobile") also uses Omniture and downloads a 50KB (19KB gzipped) JavaScript.

[IBM](http://m.ibm.com/us/en/ "IBM Mobile") sends a beacon to unica.com. The beacon URL is built by a [downloaded JavaScript file](http://www.ibm.com/common/stats/stats.js "IBM Metrics Script"). The file size is 18.6KB minified (6.8KB gzipped).

[Microsoft](http://m.microsoft.com "Microsoft Mobile") uses a directly embedded beacon image that hits [webtrends.com](http://webtrends.com/ "webtrends").

SAP, Canon, Asus, Toshiba, Fujitsu and Acer do not serve optimized content for mobile - or at least I was not able to find their mobile site given the methods described in the beginning.
