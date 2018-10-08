+++
title =  "OAuth in Vue app"
date = 2018-10-05T11:53:14+02:00
featured_image = ""
description = ""
tags = ['oauth']
categories = ['vue']
+++

<!-- more -->

Install version 3 of vue-cli

    sudo npm install -g @vue/cli

    vue --version


Bootstrap the application with vue-cli

    vue create app-name
    cd app-name
    npm run serve

Add the router

    vue add router

Install OAuth library

    npm install --save auth0-js

Create a file auth.js inside a folder called utils

    // utils/auth.js
    import auth0-js from 'auth0-js'
    const webAuth = auth0.WebAuth({
      domain, clienID, ...  -> get from auth0.com dashboard addons tab
      })
    const login = () => {
      webAuth.authorize()
    }
    export {
      login
    }

    Note:

    redirectUri: process.env.NODE_ENV === 'production' ? 'http://eelp_form_builder.eelp.com/callback' : 'http://localhost:8080/callback',


Use it

    import login from '../utils/auth'

    <button type='button' @click='login()'><Login</button>

    methods: {
      login () {
        login()
      }
    }

Declare callback url in auth0.com app

Parse server response

Continue in Udemy course Secure Your VueJs Applications With Auth0


NGINX New server block

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
