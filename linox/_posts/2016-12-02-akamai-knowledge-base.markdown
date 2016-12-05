---
layout: post
title:  "Akamai (small) knowledge base"
date:   2016-12-02 20:40:17 +0100
comments: true
categories: Akamai
---
- Vary header without Cachecontrol header: Akamai wil not cache your data!
- Get the Akamai "debug" headers:
{% highlight bash %}
curl -m 10 -D - -o /dev/null -s --compressed -H "Pragma: akamai-x-get-cache-key" -H "Pragma: akamai-x-cache-on" -H "Pragma: akamai-x-cache-remote-on" -H "Pragma: akamai-x-get-true-cache-key" http://i3.microsoft.com/library/svy/broker.js
{% endhighlight %}
