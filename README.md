# Readme

Source code of my blog. The blog's url is [https://benedictnghk.github.io/](https://benedictnghk.github.io/)
Please note: the disqus comment system is unavailable for IP addresses from Mainland China because of some obvious reasons.

themes: [overdose](https://github.com/HyunSeob/hexo-theme-overdose)
Fixed some bugs. Google analytics and disqus are no longer available for the original theme because the source code embedded in the theme is outdated. Let's assume the root directory is your overdose directory.

For google analytics: modify the code in /layout/includes/head.pug, and find the snippet of code of google analytics, replace it to:

```pug
if theme.google_analytics
  script(async src='https://www.googletagmanager.com/gtag/js?id=' + theme.google_analytics)
  script. 
    <!-- Google tag (gtag.js) -->
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', '#{theme.google_analytics}');
    
```

For disqus: modifiy the code in /layout/includes/article.pug. The url should be different, you can find it on your disqus account. In my case, it is:

```pug
  if config.disqus_shortname
    #disqus_thread
    script.
      (function() { // DON'T EDIT BELOW THIS LINE
      var d = document, s = d.createElement('script');
      s.src = 'https://https-benedictnghk-github-io.disqus.com/embed.js';
      s.setAttribute('data-timestamp', +new Date());
      (d.head || d.body).appendChild(s);
      })();
    noscript Enable JavaScript to see comments.
```

Besides, I've added a new layout, /layout/includes/no-comments.pug in case that for some pages I don't want to they be commented. It is simply derived from the article.pug but commented out the code of disqus.
