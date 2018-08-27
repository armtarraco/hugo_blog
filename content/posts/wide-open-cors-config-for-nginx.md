+++
title =  "Wide Open Cors Config for Nginx"
date = 2018-07-13T13:25:04+02:00
featured_image = ""
description = ""
tags = ['cors']
categories = ['nginx', 'devops']
+++

<!--more-->

Source: https://gist.github.com/michiel/1064640

Add this block to location section of server configuration.
Remember allow the necessary headers

    if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Origin' $http_origin always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, DELETE';
        add_header 'Access-Control-Allow-Headers' 'x-user-token,x-flavour,Authorization,Origin,Accept,DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
        return 204;
     }
     if ($request_method = 'POST') {
        add_header 'Access-Control-Allow-Origin' $http_origin always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, DELETE';
        add_header 'Access-Control-Allow-Headers' 'x-user-token,x-flavour,Authorization,Origin,Accept,DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
     }
     if ($request_method = 'GET') {
        add_header 'Access-Control-Allow-Origin' $http_origin always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, DELETE';
        add_header 'Access-Control-Allow-Headers' 'x-user-token,x-flavour,Authorization,Origin,Accept,DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
     }
     if ($request_method = 'DELETE') {
        add_header 'Access-Control-Allow-Origin' $http_origin always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, DELETE';
        add_header 'Access-Control-Allow-Headers' 'x-user-token,x-flavour,Authorization,Origin,Accept,DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
     }


https://gist.github.com/Stanback/7145487

inside your **location** block(s):

    include /etc/nginx/cors_support.conf;

CORS configuration configuration file

    # /etc/nginx/cors_support.conf
    set $cors false;
    if ($http_origin ~ '^https?://(localhost:?\d*|eelp_form_builder\.eelp\.com)') {
        set $cors true;
    }

    if ($cors = true) {
        add_header 'Access-Control-Allow-Origin' $http_origin always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'x-user-token,x-flavour,Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Requested-With' always;
        # required to be able to read Authorization header in frontend
        #add_header 'Access-Control-Expose-Headers' 'Authorization' always;
    }

    if ($request_method = 'OPTIONS') {
        add_header 'Access-Control-Allow-Origin' $http_origin always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'x-user-token,x-flavour,Accept,Authorization,Cache-Control,Content-Type,DNT,If-Modified-Since,Keep-Alive,Origin,User-Agent,X-Requested-With' always;
        # Tell client that this pre-flight info is valid for 20 days
        add_header 'Access-Control-Max-Age' 1728000;
        add_header 'Content-Type' 'text/plain charset=UTF-8';
        add_header 'Content-Length' 0;
        return 204;
    }
