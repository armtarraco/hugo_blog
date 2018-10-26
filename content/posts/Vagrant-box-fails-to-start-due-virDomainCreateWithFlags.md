+++
title =  "Vagrant Box Fails to Start Due VirDomainCreateWithFlags"
date = 2018-10-26T09:22:22+02:00
featured_image = ""
description = ""
tags = ['vagrant']
categories = []
+++

Source: http://ask.xmodulo.com/network-default-is-not-active.html

    vagrant up
    => Call to virDomainCreateWithFlags failed: The requested information is invalid: network 'rails_apps1' is not active

    sudo virsh net-list --all
    =>
     Nombre               Estado     Inicio automático Persistente
    ----------------------------------------------------------
     default              activo     si            si
     rails_apps1          inactivo   no            si
     vagrant-libvirt      activo     no            si

Solution:

Try to start network

    sudo virsh net-start rails-apps1

If you encounter the following error, go to the next solution.

    internal error: Network is already in use by interface virbr0

Remove that bridge as follows.

    sudo ifconfig virbr0 down
    sudo brctl delbr virbr0

and try to start the network again.

