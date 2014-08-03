---
layout: post
title: AWS and Travis-CI
description: "Encrypt your AWS secrets (including PEM file) for consumption by Travis-CI."
modified: 2014-08-03 09:16:16 -0700
tags: [aws,travis-ci,secrets]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
comments: true
share: true
---

Check out this gist for an example of how to show gist:

{% highlight bash %} 
TRAVIS_CI_SECRET=`cat /dev/urandom | head -c 10000 | openssl sha1`
openssl aes-256-cbc -pass "pass:$TRAVIS_CI_SECRET" -in $1 -out ./.secret -a
 
travis encrypt TRAVIS_CI_SECRET=$TRAVIS_CI_SECRET --add --override
travis encrypt AWS_ACCESS_KEY=$AWS_ACCESS_KEY --add
travis encrypt AWS_SECRET_KEY=$AWS_SECRET_KEY --add
travis encrypt AWS_SSH_KEY=$AWS_SSH_KEY --add
travis encrypt AWS_SSH_KEY_ID=$AWS_SSH_KEY_ID --add
{% endhighlight %}

{% gist wellsie/96427f843c7570f08923 %}