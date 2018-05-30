---
layout: post
title: "Should IE7 Mobile (Still) Be Supported?"
date: 2012-04-10 22:02
comments: true
categories: [web, mobile, IE] 
---

Among the variety of mobile platforms available today, one that gets its fair share of media coverage is the Windows Phone platform. I've only used it for about a week last year (on an [HTC HD7](http://www.htc.com/uk/smartphones/htc-hd7/)) and I can't say it's  bad, although I suffered a bit being primarily exposed to Android before using it. The point I'm trying to make is that Windows Phone 7 doesn't matter in the long run and that it's harmful to take IE7 Mobile into account when developing a mobile website or webapp.

Although StatCounter is [not the most accurate](http://blog.wapreview.com/9157/) source for mobile stats, it [clearly shows](http://gs.statcounter.com/#mobile_browser-ww-monthly-201103-201203) that IE Mobile is not a force to be taken into consideration. It is not a feature phone, to say that it has JavaScript disabled  and doesn't get tracked by metrics scripts. Other studies also show that it just doesn't have enough [market share](http://blog.nielsen.com/nielsenwire/consumer/more-us-consumers-choosing-smartphones-as-apple-closes-the-gap-on-android/) compared to its competitors.

So, it's pretty clear that Microsoft missed out on the start of the smartphone boom, but they certainly have the capacity to gain users (especially in the case of [a Nokia takeover](http://techcrunch.com/2012/01/05/microsoft-nokia-smartphone-division-unit/)). Even in this case, IE7 Mobile *will not grow*. All [new released Windows Phones](http://www.nokia.com/us-en/products/phone/lumia900/specifications/) ship with [WP 7.5](http://www.microsoft.com/windowsphone/en-us/howto/wp7/start/whats-new-in-windows-phone.aspx) that comes with an IE that [has its rendering engine based on IE 9](http://en.wikipedia.org/wiki/Internet_Explorer_Mobile#Internet_Explorer_Mobile_9) as the default browser. It's almost impossible to downgrade, although I don't imagine why anyone would want to do that. 

Also, Microsoft allows existing WP 7 users to [upgrade to 7.5](http://www.microsoft.com/windowsphone/en-us/howto/wp7/basics/phone-updates.aspx) so existing users could get the new IE without having to change their device. And even if they did have to, users change their phones [a lot more often](http://www.phonearena.com/news/How-often-do-you-change-your-phone-Poll-Results_id24668) than they change their computers. This means that the meager slice of the market the IE7 Mobile has will be gone in no time.

I won't rant against IE and all its wrongs in this post. Probably every web developer on the internet has done that at one point. What I am trying to say is that supporting a non-standards compliant browser with dismal market share and practically no prospect for growth is a waste of money. 

If you do decide to drop IE support you'll end up with an improved baseline for development that is closer to the standards of today's web development ecosystem. Major sites are [dropping IE7 support on desktop](http://venturebeat.com/2011/12/30/facebook-timeline-ie7/), where it's market share is more significant. If they can afford to do that, considering their traffic, it's hard to believe that it won't be more profitable for you to do this.
