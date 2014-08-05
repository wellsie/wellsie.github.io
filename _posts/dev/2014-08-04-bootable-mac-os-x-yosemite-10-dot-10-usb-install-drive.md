---
layout: post
title: "Bootable USB Drive - Mac OS X Yosemite 10.10"
modified: 2014-08-04 19:24:58 -0700
tags: [osx,yosemite,usb,install,10.10]
image:
  feature: Sydney_Harbour_Bridge_-_View_from_Milsons_Point_(7653426404).jpg
  credit: Photographic Collection
  creditlink: http://commons.wikimedia.org/wiki/File:Sydney_Harbour_Bridge_-_View_from_Milsons_Point_(7653426404).jpg#file
comments: true
share: true
---


# Create OS X Yosemite Boot Disk

- Download OS X Yosemite the [beta](https://www.apple.com/osx/preview/) or [developer preview](https://developer.apple.com/osx/whats-new/)
- Open a terminal
- Insert USB Drive

{% highlight bash %}
$ /Applications/Install\ OS\ X\ Yosemite\ Developer\ Preview.app/Contents/Resources/createinstallmedia \
  --volume /Volumes/Untitled --applicationpath \
  "/Applications/Install OS X Yosemite Developer Preview.app"
{% endhighlight %}

Set value of `--volume` to your USB drive's mount point
{: .notice}

To determine drive mount point:

{% highlight bash %}
$ df -lH | grep "Filesystem"; df -lH | grep "/Volumes/*"
{% endhighlight %}


# References

- [How to Make a Bootable OS X Mavericks USB Install Drive](http://osxdaily.com/2013/06/12/make-boot-os-x-mavericks-usb-install-drive/)
- [How can I get the mount path of a USB device on OSX?](http://superuser.com/questions/429058/how-can-i-get-the-mount-path-of-a-usb-device-on-osx)