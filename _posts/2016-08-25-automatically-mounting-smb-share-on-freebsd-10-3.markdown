---
layout: post
title:  "Automatically Mounting SMB Share on FreeBSD (10.3)"
date:   2016-08-24 20:47:03 -0600
categories: smb mount share freebsd automatic
---
Coming from the Linux world, I discovered that mounting SMB shares, especially via fstab, was quite a bit different in FreeBSD.

To manually mount a share:
`mount_smbfs -I samba.mydomain.com -N //guest@samba/public /smb/public`

The `/etc/fstab` entry equivalent:
`//guest@samba/public /smb/public smbfs rw,-Isamba.mydomain.com,-N 0 0`

Option breakdown from the mount_smbfs manpage:

    -I host
        Do not use NetBIOS name resolver and connect directly to host,
        which can be either a valid DNS name or an IP address.

     -N      Do not ask for a password.  At run time, mount_smbfs reads the
             ~/.nsmbrc file for additional configuration parameters and a
             password.  If no password is found, mount_smbfs prompts for it.

Although this provides the username to use when mounting the share, it doesn't provide a password.  The `-N` option makes `mount_smbfs` not ask for a password.  I don't think there's a way to provide a password in the fstab entry, but most users will certainly need one.

If you're mounting the share as a regular user (like the very first `mount_smbfs` example), you can put the password/share info in the `~/.nsmbrc` file in your home directory.  Since our focus is on mounting it automatically at boot via `/etc/fstab`, it will get mounted during boot, and it won't check your user's home directory for the file -- it looks for `/etc/nsmb.conf`, which the `mount_smbfs` man page doesn't mention, even though it talks about an fstab entry...

`man 5 nsmb.conf` shows some examples of what your password/share entry needs to look like:

    # User specific data for FSERVER
    [FSERVER:MYUSER]
    password=$$16144562c293a0314e6e1

The manpage doesn't explicitly state this, but the server name and user name MUST be in caps, otherwise you'll get this message:
`mount_smbfs: unable to open connection: syserr = Authentication error`

You might notice that password looks strange.  That is the output of running `smbutils crypt passwordhere`, so you don't have to put your password in plain text in the file, but you can if you'd like.
