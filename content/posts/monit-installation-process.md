+++
date = "2018-05-15T09:14:01+02:00"
draft = true
title = "Monit installation process"

+++
Monit installation example for monitoring Rails application deployed with Capistrano in a CentOS Linux machine.

<!--more-->

# Installation

    sudo yum -y install monit
    sudo service monit start
    sudo service monit status
    sudo service monit enable

# Deploy of Monit configuration file

    cap prepro puma:monit:config
    => 00:00 puma:monit:config Uploading /tmp/monit.conf 100.0%
    01 sudo mv /tmp/monit.conf /etc/monit/conf.d/puma_backend.conf
    01 mv: no se puede mover «/tmp/monit.conf» a «/etc/monit/conf.d/puma_backend.conf»: No existe el fichero o el directorio

Monit configuration file must be moved to destination folder by hand due to writing restrictions to deployer user.

    ssh backend
    sudo mv /tmp/monit.conf /etc/monit.d/backend_monit.conf
    sudo monit reload

# Nginx configuration

Added to nginx template

    location /monit/ {
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_pass http://localhost:2812;
      proxy_redirect off;
      rewrite ^/monit/(.*) /$1 break;
      proxy_ignore_client_abort on;
    }

# Web server operations

Restarting:

    cap backend puma:restart

Is equivalent to local execution of

    $HOME/.rbenv/bin/rbenv exec bundle exec pumactl -S /var/www/backend/shared/tmp/pids/puma.state restart

Starting:

    cap backend puma:start

Is equivalent to local execution of

    $HOME/.rbenv/bin/rbenv exec bundle exec puma -C /var/www/eelp_backend/shared/puma.rb --daemon

Stopping:

    cap backend puma:stop

Is equivalent to local execution of

    $HOME/.rbenv/bin/rbenv exec bundle exec pumactl -S /var/www/eelp_backend/shared/tmp/pids/puma.state stop