+++
title =  "Api load test with Taurus"
date = 2018-06-11T16:54:11+02:00
featured_image = ""
description = ""
tags = []
categories = ['test']
+++


## Instal Taurus

http://gettaurus.org/docs/Installation/#CentOS

Excerpt

    python -v
    => 2.7.5
    sudo yum -y install python-pip
    sudo yum install python-devel
    sudo pip install bzt

Add installation folder to path if necessary

    vi ~/.bash_profile 

Add line

    export PATH=$PATH:/root/.local/bin


## Test example

Customize as needed

    test-sign_in.yaml

    execution:
    - concurrency: 50
      throughput: 50
      ramp-up: 30s
      hold-for: 90s
      steps: 1
      scenario: test-sign_in

    scenarios:
      test-sign_in:
        timeout: 10s
        retrieve-resources: false
        store-cache: false
        store-cookie: false
        default-address: http://192.168.33.11:3000
        headers:
          Accept-Language: 'es-Es,ru;q=0.8,en-US;q=0.6,en;q=0.4'
          Content-Type: 'application/json'
        requests:
        - url: '/api/v2/user/sign_in'
          method: POST
          headers:
            Content-Type: application/json
          body:
            email: "67webs@gmail.com"
            password: "123123123"


## Execution

    bzt test-sign_in.yaml

