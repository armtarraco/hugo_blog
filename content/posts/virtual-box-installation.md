+++
title = "Virtual Box Installation"
date = 2018-11-26T10:31:22+01:00
featured_image = ""
description = ""
tags = ['vurtualbox']
categories = ['devops']
+++

# VIRTUAL BOX INSTALLATION

https://www.if-not-true-then-false.com/2010/install-virtualbox-with-yum-on-fedora-centos-red-hat-rhel/


https://forums.fedoraforum.org/showthread.php?289271-vboxdrv-kernel-module-is-not-loaded

Response 1
----------
I go to www.virtualbox.org. I click downloads. I choose the rpm for Fedora. I store it somewhere. I then also download the extension pack (mentioned in second paragraph on the download page.) I install that.

I make sure that I have `kernel-devel` installed. If one has a PAE kernel, they may have to install it with `yum install kernel-pae-devel` rather than `kernel-devel`. I haven't used it in awhile, so not sure if they fixed it or don't consider it a bug. Make sure your `kernel-devel` package matches your `uname -r`

    $ yum list kernel

    Paquetes instalados
    kernel.x86_64              2.6.32-696.6.3.el6     @koji-override-1/6.9
    kernel.x86_64              2.6.32-696.10.2.el6    @updates
    kernel.x86_64              2.6.32-696.16.1.el6    @updates
    kernel.x86_64              2.6.32-696.18.7.el6    @updates
    kernel.x86_64              2.6.32-696.30.1.el6    @updates
    Paquetes disponibles
    kernel.x86_64              2.6.32-754.6.3.el6     updates

    $ uname -r
    4.15.17-200.fc26.x86_64

    $ sudo rpm -qa | grep -i virtualbox
    VirtualBox-5.1-5.1.36_122089_fedora26-1.x86_64


    rpm -q kernel-devel (or kernel-PAE-devel)

Might show more than one, just make sure that at least one of the listed ones matches the output of `uname -r`.

Now, go to where you downloaded the VirtualBox rpm.

As root or with sudo

    yum install VirtualBox-whatever.rpm

Yum should pull in `whatever` is needed.

Now, `/etc/init.d/vboxdrv` setup should work.

Assuming that does, start VBox once as root or with sudo

    sudo VirtualBox

Click preferences, extensions, browse to where you put the extension pack and install it. Close it and run it as normal.

If there's a newer version, it will notify you on startup. If you upgrade a kernel, you will have to once again run /etc/vboxdrv setup (or service vboxdrv setup, both work.) 

The disadvantage is that if you upgrade your kernel and forget to run vboxdrv setup you'll get an error and then have to run it. With akmod, if it works, it's transparent, but if it doesn't work, which, judging from these forums, seems to happen from time to time, at least you're not wasting time trying to figure out what's wrong.


Response 2
----------
What's wrong with just installing the virtualbox repository and install the official VirtualBox with yum?, I've always had luck with that one and it's much easier than all this rigmarole with scripts:

https://www.virtualbox.org/wiki/Linux_Downloads

go to the bottom of the page and read and follow the section after this text:
Note that importing the key is not necessary for yum users (Oracle Linux/Fedora/RHEL/CentOS) when using one of the virtualbox.repo files from below as yum downloads and imports the public key automatically!
and install the virtualbox.repo file they give here:

http://download.virtualbox.org/virtu...irtualbox.repo

into `/etc/yum.repos.d`

and then just run

    yum install VirtualBox-4.2 dkms

the critical step that makes the kernel specific vboxdrv.ko and other modules should be done automatically by the installation script in the rpm. But the extension pack is not in the repo, you'd get that from: https://www.virtualbox.org/wiki/Downloads

Note also, in Linux and UNIX in general, it's very possible to install a new executable and find that running the command in a terminal results in "Command not found".
This is because the shell's hash string is now stale and needs update to find the new command. If your shell is tcsh, you'd run "rehash" but the default shell in Fedora is
bash which uses "hash" or you could just restart a fresh shell




CONVERT VIRTUALBOX BOXES
========================

https://www.reddit.com/r/linux/comments/72hj9v/fedora_26_virtual_box_kernel_not_loaded_error_fix/

    qemu-img convert -f vdi -O qcow2 vm.vdi vm.qcow2

