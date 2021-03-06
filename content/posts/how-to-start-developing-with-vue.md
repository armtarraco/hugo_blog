+++
title =  "How to Start Developing With Vue"
date = 2018-07-13T11:53:14+02:00
featured_image = ""
description = ""
tags = []
categories = ['node', 'vue']
+++

<!-- more -->

# Install node and npm

    sudo curl -sL https://rpm.nodesource.com/setup_8.x | sudo bash -
    sudo yum install -y nodejs
    node -v
    => v8.11.2
    npm -v
    => 5.6.0

# Install vue-cli 2

    sudo npm install -g vue-cli
    =>
    npm WARN deprecated coffee-script@1.12.7: CoffeeScript on NPM has moved to "coffeescript" (no hyphen)
    /usr/bin/vue -> /usr/lib/node_modules/vue-cli/bin/vue
    /usr/bin/vue-list -> /usr/lib/node_modules/vue-cli/bin/vue-list
    /usr/bin/vue-init -> /usr/lib/node_modules/vue-cli/bin/vue-init
    + vue-cli@2.9.6
    added 252 packages in 10.868s

# Bootstrap the application with vue-cli

    vue init webpack app_name
    cd app_folder
    npm install
    npm run dev
    =>
    ...
    DONE  Compiled successfully in 4591ms
    Your application is running here: http://localhost:8081

# Installing modules

    npm install axios
    npm install bootstrap-vue bootstrap
    npm install --save-dev @vue/test-utils

# In order to use object spread operator '...', at this moment is necessary to install the following plugin

https://babeljs.io/docs/en/babel-plugin-transform-object-rest-spread/

Source: https://github.com/symfony/webpack-encore/issues/254

For example,

    export default new Vuex.Store({
      state,
      mutations,
      getters,
      computed: {
        ...mapGetters(['getCounter'])
      }
    })

# Deploy

Generate dist folder to be ftp to the production server

    npm run build

# Create destination folder

    ssh pre
    cd /var/www
    sudo mkdir eelp_form_builder
    sudo chown deployer:deployer eelp_form_builder/


# HTTP server for Node

    npm install http-server -g

    http-server eelp_form_builder/ -p 8085

# nginx configuration on dashboard server

    location /eelp_form_builder/ {
        root /var/www/eelp_form_builder/index.html;
        index index.html;
        proxy_pass http://127.0.0.1:8085;
        proxy_redirect off;
        proxy_set_header Host $host;
        rewrite ^/eelp_form_builder/(.*)$ /$1 break;
    }

# New server block -> eelp_form_builder

    server {
            listen 80;
            listen [::]:80;

            root /var/www/eelp_form_builder;
            index index.html;
            server_name eelp_form_builder.eelp.com;
            location / {
              try_files $uri $uri/ /index.html;
            }
    }

# Testing

https://tighten.co/blog/its-time-to-start-testing-your-vue-components-getting-started-with-jest

https://github.com/jest-community/jest-extended
http://babeljs.io/docs/en/babel-plugin-transform-async-generator-functions
https://babeljs.io/docs/en/babel-polyfill/

# CORS enabling

If connected to a Backend Rails server, CORS must be enabled. See http://hugo.67webs.com/posts/wide-open-cors-config-for-nginx/

# How to view app variables in the browser

in main.js

    window['appMode'] = process.env.NODE_ENV
