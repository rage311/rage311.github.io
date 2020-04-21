---
layout: post
title:  "Locking Down a New VPS - Part 1"
date:   2016-04-07 16:30:23 -0600
categories: vps cloud server linux firewall
---
### What?
So you've just gotten yourself a shiny new Virtual Private Server in the [cloud](https://www.stickermule.com/marketplace/3442-there-is-no-cloud).  Now you're excited to do something with it, like: a [VPN](https://openvpn.net/), [Minecraft server](http://cuberite.org/), [blog](https://ghost.org/), [Mumble](https://wiki.mumble.info/wiki/Main_Page) server, etc., etc.  But before you even think about getting any of that setup, you need to protect your server from would-be intruders.  Although it's unlikely somebody will be targeting you specifically, there are plenty of massive botnets out there probing the entire internet for vulnerable servers to add to their virtual army.

### Bring Your System Up to Date
First things first, make sure all of the packages on your VPS are up-to-date.  It's likely that the VPS install that you have is behind on patches -- which includes patches for security vulnerabilites and bugs.

Debian/Ubuntu:

    # apt-get update && apt-get dist-upgrade

### SSH
Since a VPS is remote, and the user needs some way to connect to and manage it, SSH is usually enabled by default.  SSH is no doubt one of the biggest targets, since it's so ubiquitous, and it provides extensive access to vast portions of a machine.  The attacks against SSH are _usually_ just simple bruteforce attacks of trying username/password combinations until it gets in.  Since the root user is the most common super user name in the *nix world, and it provides the most privileged access on a machine, that's the username that gets tried the most.

For this reason, allowing remote SSH logins for the root user is a bad idea -- especially with password-based authentication.  It's better to create a second user and allow it to perform actions as the super user via sudo.  Your VPS might already be setup with a \"regular\" user for this purpose.  If not, something along the lines of

    # useradd -m -G wheel yourusernamehere
    # passwd yourusernamehere

should create a user named \"yourusernamehere\", add that user to the \"wheel\" group, create a home directory, and finally set a password for that user.

Then, allow users in the group \"wheel\" to run commands as the super user via sudo.  First, obviously, install the sudo package from your distro's repositories via your package manager, if it's not already installed.  For Debian/Ubuntu-based distros:

    # apt-get install sudo

then run

    # visudo

to edit the sudoers file and uncomment the line:

    # %wheel ALL=(ALL) ALL

Now we'll have to generate an [SSH key](https://wiki.archlinux.org/index.php/SSH_keys) on your _local machine_, as your user, that you want to use to SSH into the server.  The below command will generate an SSH key of type RSA that is 4096 bits:

    $ ssh-keygen -b 4096 -t rsa

You can choose whether or not to encrypt your private key with a password here, and it will ask you where to save it -- the default is:

    ~/.ssh/id_rsa

There are of course other options for key generation that you can review in the ssh-keygen manpage.

You will have an id\\_rsa file and an id\\_rsa.pub file.  The .pub file is your _public_ key, which has the contents that we will need to put onto the remote server in order to allow your user to login.

On the _remote machine_, we'll need to create a file called authorized_keys in the regular user's .ssh directory in their home folder.

    # su yourusernamehere
    $ cd ~
    $ mkdir .ssh
    $ nano authorized_keys
(or whatever your editor of choice is in place of nano).

Now paste the entire contents of your local user's id_rsa.pub file into the remote user's authorized\\_keys file.

Now that we've gone through the tedious process of setting up our SSH keys, we need to configure the SSH daemon accordingly.  For locking down SSH, our primary goals will be: disabling root user logins, disabling password-based authentication, and enabling key-based authentication.  Changing the port that the SSH service binds to can also provide _some_ added security by obscurity, since most botnet scripts are looking for port 22 to be open, which is the SSH default.

Let's look at the SSH daemon's configuration file.  On most machines, it will be located at:

    # nano /etc/ssh/sshd_config

The following directives should already exist in the file.  Find them and make sure they are uncommented and match the following:

    PermitRootLogin no
    PasswordAuthentication no
    ChallengeResponseAuthentication no
    UsePAM no
    Protocol 2
    RSAAuthentication yes
    PubkeyAuthentication yes

Save and quit.  Now if you restart the SSH daemon, you should only be allowed to login as the remote regular user with the public key that you copied from your local machine.

    # systemctl restart sshd.service

or in the case of an older Debian installation (or FreeBSD), it should be:

    # service sshd restart

If all goes well, you won't be locked out of your VPS... hopefully you have console access anyway via a web GUI.

Since this post became a little longer than I anticipated, I'm going to split it into multiple parts.  For part 2, I plan to focus on firewall configuration.
