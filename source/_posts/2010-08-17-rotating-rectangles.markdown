--- 
wordpress_id: 58
layout: post
title: Rotating Rectangles
date: Tue Aug 17 14:21:23 +0300 2010
categories: 
  slug: javascript
  title: javascript
  autoslug: javascript
wordpress_url: http://improve.ro/?p=58
---
Working on the [maps48 project](http://maps48.sourceforge.net/ "Maps48"), one of the problems that needed solving was calculating the amount of map space needed to be retrieved to allow rotating the map without introducing any blank space in the viewport. As you can see from the diagram below what we actually need is to get the rotated rectangle's horizontal and vertical projection as the new width and height.The black border represents the viewport. If we were to only have an image the size of the viewport and rotate it, we'd end up with something like the orange rectangle. Notice the blank spaces in the viewport :). The solution is to resize (or import a larger bit of the map, in our case) the image - the pink rectangle - so that when rotated it fills up the viewport entirely, as does the green rectangle.

[![Problem description](http://improve.ro/wp-content/uploads/2010/08/rectangles.png "Rotating rectangles problem description")](http://improve.ro/wp-content/uploads/2010/08/rectangles.png)

This can be easily achieved through some basic trigonometry. The key to this are two angles: alpha, the angle the rectangle was rotated by and beta, the angle the diagonal makes with the width.

<!-- more -->

[![Problem diagram](http://improve.ro/wp-content/uploads/2010/08/rectangles-2.png "Problem diagram")](http://improve.ro/wp-content/uploads/2010/08/rectangles-2.png)

First,  I determine the angle the diagonal makes with the baseline (the rectangle's width):

[![Equation 1](http://improve.ro/wp-content/uploads/2010/08/CodeCogsEqn-1.png "Equation 1")](http://improve.ro/wp-content/uploads/2010/08/CodeCogsEqn-1.png)

Now, we know that:

[![Equation 2](http://improve.ro/wp-content/uploads/2010/08/rot-height-1.png "Equation 2")](http://improve.ro/wp-content/uploads/2010/08/rot-height-1.png)

This way we can determine the new height. The new width can be determined in the same way:

[![Equation 3](http://improve.ro/wp-content/uploads/2010/08/CodeCogsEqn-2.png "Equation 3")](http://improve.ro/wp-content/uploads/2010/08/CodeCogsEqn-2.png)

And here's a translation of this in JavaScript:

{% highlight javascript %}
function getRotatedDimensions(width, height, rot) {
    // rotation angle in radians
    var alpha = rot * Math.PI / 180;
    // the angle made by the diagonal and the width in radians
    var beta = Math.atan(height / width);
    // diagonal length
    var diagonal = Math.sqrt(width * width + height * height);
    var newHeight = Math.sin(alpha + beta) * diagonal;
    var newWidth = Math.cos(beta - alpha) * diagonal;
    return { "width": newWidth, "height": newHeight };
}
{% endhighlight %}

This was inspired by [Adrian](https://beradrian.wordpress.com/ "Adrian Ber")'s post about [overlapping rectangles](https://beradrian.wordpress.com/2010/08/02/overlapping-rectangles/ "Overlapping Rectangles"). The graphics above were generated with Photoshop and the formulas were built using the excellent [CodeCogs online Equation Editor](http://www.codecogs.com/latex/eqneditor.php "Equation Editor").
