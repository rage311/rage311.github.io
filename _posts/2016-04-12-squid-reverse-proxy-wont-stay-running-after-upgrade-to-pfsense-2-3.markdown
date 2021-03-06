---
layout: post
title:  "Squid Reverse Proxy Won't Stay Running After Upgrade to pfSense 2.3"
date:   2016-04-12 16:30:23 -0600
categories: pfsense squid reverse proxy freebsd unix
---
### Overview
After upgrading my pfSense box from 2.2.4 to 2.3, the squid reverse proxy service wouldn't stay running.

### Procedure
I couldn't seem to get any useful debugging information from the web GUI to figure out what was going wrong.  I uninstalled, rebooted, and reinstalled the package to no avail.

I ended up installing the sudo package so I could ssh into the machine and run commands as the super user in order to see the log files.  There weren't any logs in `/var/log/squid/`, but the `/var/log/system.log` provided some insight.  The log file was littered with this message:
        
    Apr 13 10:03:17 pfsense (squid-1): UFSSwapDir::openLog: Failed to open swap log.

I found `swap.state` and `swap.state.last-clean` files in the `/var/squid/cache` directory.  I wiped that whole directory with an `rm -rf /var/squid/cache/*`, restarted the service and... it still wouldn't stay running.  So, in some frustration, I just wiped every file that I could see that was squid-related:

    # rm -rf /var/log/squid
    # rm -rf /var/squid
    # rm -rf /usr/local/etc/squid

And after another service restart, it worked.  I didn't nail it down to exactly which file(s) had the issue, so you may want to do a little more detective work if you have lots of data in these directories.  I had just started using the reverse proxy function a few days prior, so I didn't have much to lose. 
