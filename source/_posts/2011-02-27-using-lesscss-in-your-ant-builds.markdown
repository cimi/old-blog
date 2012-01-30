--- 
wordpress_id: 141
layout: post
title: "Using LessCSS in Your Ant Builds"
date: Sun Feb 27 22:22:18 +0200 2011
categories: [web, css, java]
wordpress_url: http://improve.ro/?p=141
---

I created a fork of the [Java flavoured](http://www.asual.com/blog/lesscss/2009/11/05/less-for-java.html) port of [LessCSS](http://lesscss.org/) to enable including it in an Ant build processes. You can find the [source on GitHub](https://github.com/cimi/lesscss-engine). The reason I needed to do this and not use the [servlet](https://github.com/asual/lesscss-servlet) to build them on the fly (and cache them, of course :) is because one of the envrionments I'm using is a CMS setup that imposes a lot of constraints on how you organize your resources. For the moment, generating the files on my development machine and uploading them manually in the CMS seemed the better option, as [CSS explosion](http://news.ycombinator.com/item?id=2146580) was iminent :).There wasn't much to modify, I just added a class that implements the Task interface from Ant. You can alias it in your buildfile like this:

``` xml
    <target name="build.css">
        <taskdef name="lesscss" classname="com.asual.lesscss.LessEngineTask" classpathref="build.aux" />
        <property name="css.dir" value="[your_css_dir]" />
        <lesscss input="${css.dir}/[less1]" output="${css.dir}/[css1]" />
        <lesscss input="${css.dir}/[less2]" output="${css.dir}/[css2]" />
    </target>
```

The classpath reference must contain at least Mozilla Rhino (js-1.6R7.jar) and the Apache Commons Logging library.You can integrate this with Eclipse by adding your buildfile to the Ant view. When you double-click the build.css task, the final CSS files get generated. If you have auto-refresh enabled in Eclipse, you can see your updates immediately, even if your server is running.To build the project you need to have Maven installed. If you don't/can't want to do that, you can [download a prebuilt one](https://github.com/downloads/cimi/lesscss-engine/lesscss-engine-1.0.41.jar). I plan to add compression support for this, using the YUICompressor, as it is implemented in the servlet version of the Java port.
