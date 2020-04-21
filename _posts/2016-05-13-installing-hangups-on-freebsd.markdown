---
layout: post
title:  "Installing hangups on FreeBSD"
date:   2016-05-13 21:11:42 -0600
categories: freebsd hangups hangouts python
---
### How-to

Since I'm not very familiar with the python environment, and it took me a little while to figure this out, I thought I'd do a quick write-up of the install process of hangups.  It requires python 3.x, which isn't the default version for FreeBSD 10.3 as of May, 2016, which was my main issue.

* `# pkg install python3` (which is the meta package for the latest (stable?) python 3.x version)
* `$ rehash` to refresh list of installed binaries
* `# python3 --version` to find out which version of python 3.x it installed
* `# pkg install py34-setuptools34` (change the 34 to whichever version of python got installed. ie if it is python 3.5, then install py35-setuptools35)
* `$ git clone https://github.com/tdryer/hangups`
* `$ cd hangups`
* `# easy_install-3.4 .`
* `$ rehash`

Done!  Run it with `hangups`.

Here's a screenshot from the github repo:
![hangups screenshot](https://raw.githubusercontent.com/tdryer/hangups/master/screenshot.png)
