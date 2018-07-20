+++
title =  "How to Start Developing With Vue"
date = 2018-07-13T11:53:14+02:00
featured_image = ""
description = ""
tags = []
categories = ['node', 'vue']
+++

<!--more-->

Install node and npm

    sudo curl -sL https://rpm.nodesource.com/setup_8.x | sudo bash -
    sudo yum install -y nodejs
    node -v
    => v8.11.2
    npm -v
    => 5.6.0

Install vue-cli

    sudo npm install -g vue-cli
    =>
    npm WARN deprecated coffee-script@1.12.7: CoffeeScript on NPM has moved to "coffeescript" (no hyphen)
    /usr/bin/vue -> /usr/lib/node_modules/vue-cli/bin/vue
    /usr/bin/vue-list -> /usr/lib/node_modules/vue-cli/bin/vue-list
    /usr/bin/vue-init -> /usr/lib/node_modules/vue-cli/bin/vue-init
    + vue-cli@2.9.6
    added 252 packages in 10.868s

Bootstrap the application with vue-cli

    vue init webpack app_name
    cd app_folder
    npm install
    npm run dev
    =>
    ...
    DONE  Compiled successfully in 4591ms
    Your application is running here: http://localhost:8081

Installing modules

    npm install axios
    npm install bootstrap-vue bootstrap

In order to use object spread operator '...', at this moment is necessary to install the following plugin

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
