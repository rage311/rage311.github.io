---
layout: post
title:  "Choosing a Linux Distribution"
date:   2016-04-07 10:09:08 -0600
categories: linux distro distribution choosing
---
### Overview
When a friend asked for some guidance in choosing a Linux distribution for an older, less powerful laptop, I decided to do some reflection.

### Traditional Choices
While I've used [Linux Mint MATE](https://www.linuxmint.com/rel_rosa_mate_whatsnew.php) as a recommendation for low-power desktop usage situations before, I've also become frustrated with \"stable\" distributions like it in a desktop setting.  The same goes for Ubuntu and Debian (and most other traditional distributions).

My issue with these \"stable\" distributions is that it makes it much more difficult to get up-to-date packages.  You have to start adding, and trusting, various PPAs (Personal Package Archives), with some command line fu, just to be able to get somewhat recent software -- such as video drivers ([xorg-edgers](https://launchpad.net/~xorg-edgers/+archive/ubuntu/ppa) comes to mind), [WINE](https://winehq.org), browsers...  On the other hand, these distributions are fine in the server space, where version and API stability make more sense, with the only updates being security and bugfix patches.

### Stability
There is a misconception regarding the word \"stable\" when it comes to Linux distributions.  Stable is what you want, right?  Stable means no bugs, crashes, or broken systems?  Sure, it usually means that too.  As I mentioned above, they use packages that are usually out of date at the time of the distribution's release.  These package versions get used for a very long time -- sometimes until the end of the distribution's support for that release, only back-porting bugfixes and security patches.  In those terms, a rolling release distribution isn't stable... maybe \"stale\" is a better term for it then?

Okay, so where do we look now?

### Rolling Release Distributions
Rolling release distributions don't have annual/semi-annual/[seemingly-arbitrarily-chosen-interval](https://wiki.debian.org/DebianReleases) releases where software packages get frozen in time just before a release happens.  There's no such thing as a \"release\" with a rolling release distribution.  You install it once, and continually update your system with familiar package updates.  This includes all of the software on your system, including: base system utilities, desktop environments, browsers, GRUB, WINE, and even the kernel.  Package maintainers pull in the latest upstream source code, apply minimal modifications to make sure it suits their distribution's configuration, and after an appropriate time in a \"testing\" repository, they move it to the primary repository.  This means that you'll get the latest software versions usually within days or weeks of the upstream release instead of months or more.

Wouldn't that make for a less stable system?  Not in my experience.  I had more issues when I was running Kubuntu or Fedora than I've had in several years of running [Arch Linux](https://www.archlinux.org/).  Just because a package is new, it doesn't mean it's unstable.  Packages get thoroughly tested in the \"testing\" repo of Arch, and I've seen very few bugs make it through to affect me.  In my opinion, the advantages of having up-to-date software with a rolling release far outweigh any perceived disadvantages for a desktop scenario.

Some of the more popular rolling release distributions are: [Arch Linux](https://www.archlinux.org/) (and derivatives: [Antergos](https://antergos.com/), [Manjaro](https://manjaro.github.io/), and [more](https://wiki.archlinux.org/index.php/Arch_based_distributions_(active))), [Gentoo](https://www.gentoo.org/), [Alpine](http://www.alpinelinux.org/), and [PCLinuxOS](http://www.pclinuxos.com/) (partial rolling?).

### Choosing A Distribution
For me, choosing a rolling release distribution was easy.  I wanted binary packages (as opposed to compiling everything myself), [good documentation](https://wiki.archlinux.org/), [a sizeable community for support](https://bbs.archlinux.org/), and [extensive](https://www.archlinux.org/packages/) [software availability](https://aur.archlinux.org/).  The down- (and up-) side to Arch is the no-hand-holding [command line install](https://wiki.archlinux.org/index.php/Beginners%27_guide).  It's not a graphical, menu-driven installer that you might be used to from other mainstream distributions, but it really makes you dig into the internals and learn more about your OS and what goes into it to create a complete, usable system.

### Antergos
I know not everybody will want to roll up their sleeves and get dirty with an Arch install from scratch... and it can be exhausting to do for multiple machines.  Fortunately for those people, and myself occasionally, [Antergos](https://antergos.com/) exists.  Antergos provides a live USB (with [GNOME](https://www.gnome.org/)) to try it before you install, a graphical installer with your choice of 6 desktop environments, a set of reasonable default software for common tasks, and the vanilla Arch repos (which makes the amazing Arch wiki more applicable).  Of those 6 desktop environments to choose from, you have 3 lightweight options: [MATE](http://mate-desktop.com/), [Xfce](http://www.xfce.org/), and [Openbox](http://openbox.org/).

I think the out-of-the-box defaults for MATE, Xfce, and Openbox are not particularly attractive.  Conversely, the Antergos versions of these are beautiful and polished.  It's obvious they've spent quite a bit of time customizing and theming these environments to make them look great.

### Testing the Lightweight Options
I created three separate Virtualbox VMs to conduct surface level tests of each of these environments.  I couldn't figure out a way to install all three desktop environments in one VM WITH the respective Antergos themes and configurations for them, hence the separate VMs.  I poked around in the default systems for a few minutes to see what they were like, and I also checked the base RAM usage for each.  The difference in RAM usage was so small, that it wasn't a factor in the conclusion -- they were all between 260MB and 300MB.

Although any of these three would be perfect for someone in need of a lightweight desktop environment, in my opinion, the Antergos version of MATE stands out a bit above the rest.  It's gorgeous, functional, and using it just feels good.

### Conclusion
Considerations for my recommendation to my friend: lower-spec older laptop (Athlon dual core with 2GB DDR2), he's coming from Windows, he'd like to do a little light Steam/Blizzard gaming, and an easy install ending in sane defaults (since he has no real Linux experience).  My final recommendation to him seemed like an easy one -- Antergos with MATE.

### Screenshots
Here are some screenshots from each of the desktop environments' default configurations:

### MATE
![MATE](/content/images/2016/04/ocss_2016-04-06_20-50-20.png)
RAM usage (and I merged the two panels into one):![MATE RAM Usage](/content/images/2016/04/ocss_2016-04-07_07-55-15.png)

### Xfce
![Xfce](/content/images/2016/04/ocss_2016-04-06_22-51-23.png)
RAM usage:![Xfce RAM Usage](/content/images/2016/04/ocss_2016-04-07_07-57-09.png)
### Openbox
![Openbox](/content/images/2016/04/ocss_2016-04-07_07-34-22.png)
![Openbox RAM Usage](/content/images/2016/04/ocss_2016-04-07_07-50-30.png)
