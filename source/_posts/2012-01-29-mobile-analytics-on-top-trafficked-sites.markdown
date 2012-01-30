---
layout: post
title: "Mobile Analytics on Top Trafficked Sites"
date: 2012-01-29 23:16
comments: true
categories: 
---

This is a short analysis on how web analytics are implemented across the top ten trafficked shopping websites, according to Alexa. I couldn't find any mobile site traffic rankings, so I took the desktop ones and accessed them with a mobile user agent.

<table>
	<thead>
		<tr>
			<td>Rank</td>
			<td>Company</td>
			<td>Mobile Website</td>
			<td>Analytics Provider</td>
			<td>Size of JavaScript</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1.</td>
			<td>Amazon</td>
			<td>Yes</td>
			<td>JavaScript</td>
			<td>5KB (2KB)</td>
		</tr>
		<tr>
			<td>2.</td>
			<td>eBay</td>
			<td>Yes</td>
			<td>? (probably server-side)</td>
			<td>0KB</td>
		</tr>
		<tr>
			<td>2.</td>
			<td>eBay</td>
			<td>Yes</td>
			<td>? (probably server-side)</td>
			<td>0KB</td>
		</tr>
	</tbody>
</table>

Amazon uses inline JavaScript to build the URL of an analytics beacon. The code is minified and the total size (including script, noscript, img and a div tag) is 5KB. This comes to about 2KB gzipped.

eBay and Walmart do not seem to have any form of analytics code sent on the client side. Probably some form of tracking is done on the server, it being transparent to the client.

BestBuy uses two tracking images for analytics. They are directly included in the source, so there is no extra JavaScript to download and execute.

Groupon has separate sites for enhanced and featured devices. The one for enhanced uses the 13KB Google Analytics script, the one for featured devices uses rum.js, a 3KB script that generates the beacon URL.

IKEA has its analytics code bundled inside it's 50KB (zipped) JavaScript file. The size of the analytics code is 32KB minified which becomes 4KB gzipped.

Target uses Omniture for tracking and includes a 47KB (17KB gzipped) JavaScript in the page.

Newegg uses coremetrics.com and embeds a beacon image directly in the page.
