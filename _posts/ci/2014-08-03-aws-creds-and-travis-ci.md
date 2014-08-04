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

# Summary

Travis CI allows you to store encrypted environment variables in your .travis.yml file. When Travis CI kicks off a build the environment variables in .travis.yml are decrypted and exported. If you need to encrypt something more substantial, like a PEM file for access AWS you can encrypt the file with a symmetric key and store the key in an encrypted.

**tl;dr:** Check out this [gist](https://gist.github.com/wellsie/96427f843c7570f08923) for a shell script that wraps up these concepts.
{: .notice}


# Prerequisites

- A github repository setup with [Travis CI](http://docs.travis-ci.com/user/getting-started/)
- a sane ruby environment, I use [rvm](https://rvm.io)
- [CLI and Ruby client library for Travis CI](https://rubygems.org/gems/travis)


# Encrypting secrets for Travis CI

## Travis CI CLI

The [travis gem](https://rubygems.org/gems/travis) makes working with your `.travis.yml` easy. 

To create a `.travis.yml` file:

{% highlight bash %}
travis init
{% endhighlight %}

To create an entry for an encrypted environment variable:

{% highlight bash %}
travis encrypt MY_ENV_VAR=SOME_VALUE --add --override
{% endhighlight %}

**tip:** `add` updates your `.travis.yml` and `override` removes all entries
{: .notice}

## Encrypting AWS environment variables

I like to pull the AWS variables from my environment:

{% highlight bash %} 
travis encrypt AWS_ACCESS_KEY=$AWS_ACCESS_KEY --add
travis encrypt AWS_SECRET_KEY=$AWS_SECRET_KEY --add
travis encrypt AWS_SSH_KEY=$AWS_SSH_KEY --add
travis encrypt AWS_SSH_KEY_ID=$AWS_SSH_KEY_ID --add
{% endhighlight %}

To encrypt a PEM file create first create a symmetric key:

{% highlight bash %} 
export TRAVIS_CI_SECRET=`cat /dev/urandom | head -c 10000 | openssl sha1`
{% endhighlight %}

Now the encryption piece:

{% highlight bash %}
openssl aes-256-cbc -pass "pass:$TRAVIS_CI_SECRET" -in ~/.ssh/travisci-aws.pem -out ./.secret -a
{% endhighlight %}

Add the secret to your `.travis.yml` file:

{% highlight bash %} 
travis encrypt TRAVIS_CI_SECRET=$TRAVIS_CI_SECRET --add
{% endhighlight %}

On the Travis CI side to decrypt the file add this to your `.travis.yml`:

{% highlight yaml %} 
before_script:
- openssl aes-256-cbc -pass "pass:$TRAVIS_CI_SECRET" -in ./.secret -out ./travisci-aws.pem -d -a
{% endhighlight %}

BOOM! Your AWS secrets and PEM file are available to you in your Travis CI build run.

# References
- [Travis CI - Getting Started](http://docs.travis-ci.com/user/getting-started/)
- [rvm](https://rvm.io)
- [Using RVM to Manage Multiple Versions of Ruby](http://misheska.com/blog/2013/06/16/using-rvm-to-manage-multiple-versions-of-ruby/)
- [CLI and Ruby client library for Travis CI](https://rubygems.org/gems/travis)
- [gistfile1.sh](https://gist.github.com/kzap/5819745)