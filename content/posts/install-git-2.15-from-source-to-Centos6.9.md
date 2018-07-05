+++
title =  "Install Git 2.15 From Source to Centos6"
date = 2018-07-05T09:06:49+02:00
featured_image = ""
description = ""
tags = []
categories = ['git', 'devops']
+++

<!--more-->

Git version in repo EPEL for Centos 6.9 is 1.7.1

If you want latest version og Git, follow the instruction at https://tecadmin.net/install-git-on-centos-fedora/

Here is a compacted version I used to update Git in my vagrant machine

    sudo yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel
    sudo yum install -y gcc perl-ExtUtils-MakeMaker
    cd /usr/src
    sudo wget https://www.kernel.org/pub/software/scm/git/git-2.15.0.tar.gz
    sudo tar xzf git-2.15.0.tar.gz
    cd git-2.15.0
    sudo make prefix=/usr/local/git all
    sudo make prefix=/usr/local/git install
    echo 'export PATH=/usr/local/git/bin:$PATH' >> /home/vagrant/.bashrc
    . /home/vagrant/.bashrc


Verify

    git --version
    => git version 2.15.0
