---
title:  "Jekyll Email Cloak"
date:   2015-01-22 23:11:00
---

Not only is today the first real day of BinaryDreamer.com, but today is the day I published my first open source gem. 
So its feeling rather good at 11pm.

Jekyll Email Cloak is an easy way to cloak email links in your Jekyll site.
Before going much farther in how it works, one thing to note is that GitHub does not allow third party plugins. 
So this solution is really only good to you if you are hosting your own Jekyll site.

Here is an example of adding a cloaked email to the footer of this site:

_includes/footer.html

{% highlight html %}
{% raw %}
<footer>
  <div class="container">
    <p class="footer-content pull-right">
      Contact me at:
      {% cloak_email ryan binarydreamer.com %}
    </p>
    <h4 class="footer-content pull-left">BinaryDreamer.com &copy; {{ site.time | date: '%Y' }}</h4>
  </div>
</footer>
{% endraw %}
{% endhighlight %}

The output for the cloak_email tag will look something like this:

{% highlight html %}
<a href="#" id="cloak_email_1883227262981104657807012683967815274" class="cloak-email " data-before="ryan" data-after="binarydreamer.com"></a>
<script type="text/javascript"> var cloak = function() { el = document.getElementById("cloak_email_1883227262981104657807012683967815274"); var after = el.dataset.after; var before = el.dataset.before; el.href = 'mailto:' + before + '@' + after; if(el.innerHTML == '') { el.innerHTML = before + '@' + after; } }(); </script>
{% endhighlight %}

The first line is an a tag with a unique id. In additon to the unique id are two data attributes and an href that is pointing to nothing. The second line is a piece of JavaScript that targets the link above and works its magic.

After the JavaScript executes the a tag will be updated like this:

{% highlight html %}
<a href="mailto:ryan@binarydreamer.com" id="cloak_email_1883227262981104657807012683967815274" class="cloak-email " data-before="ryan" data-after="binarydreamer.com">ryan@binarydreamer.com</a>
{% endhighlight %}

The project and installation instructions can be found at <a href="https://github.com/binarydreamer/jekyll-email-cloak" target="_blank">https://github.com/binarydreamer/jekyll-email-cloak</a>
