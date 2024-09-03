---
title: Add Google Analytics & Disqus to Theme
author: Benedict Ng
categories:
    - [Hexo]
---

Since many themes were developed for years then their maintenance has stopped, so the plug-in google analytics & disqus code may invalidate. My theme [overdose](https://github.com/HyunSeob/hexo-theme-overdose) is a great example. I like its UI design but some parts of its code is obsolete. This post only shows how to modify the overdose theme, because for other themes the files are different. You have to find the correct files of your theme. Let's assume the root directory is your theme directory.

## Add Google Analytics

Modify the code in /layout/includes/head.pug, and find the snippet of code of google analytics, replace it to:

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

Then add your measurement id in _config.yml.

## Add Disqus

Modify the code in /layout/includes/article.pug. The url should be different, you can find it on your disqus account. In my case, it is:

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