+++
title =  "Api load test with Taurus & jMeter"
date = 2018-06-11T16:54:11+02:00
featured_image = ""
description = ""
tags = []
categories = ['testing']
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
    export PATH=$PATH:/root/.local/bin


## Test example

Customize as needed

    test-sign_in.yaml

    execution:
      concurrency: 30
      ramp-up: 10s
      hold-for: 10s
      iterations: 10
      # throughput: 10
      # steps: 1
      scenario: test-sign_in

    scenarios:
      test-sign_in:
        timeout: 4s  #  timeout for connecting, receiving results
        store-cache: false # browser cache simulation, enabled by default
        store-cookie: false # browser cookies simulation, enabled by default
        think-time: 0s500ms  # global delay between each request
        default-address: http://192.168.33.11:3000 # http request defaults scheme, domain, port
        headers:
          Accept-Language: 'es-Es,ru;q=0.8,en-US;q=0.6,en;q=0.4'
          Content-Type: 'application/json'
        keepalive: true  # true by default, applied on all requests in scenario
        retrieve-resources: false  # true by default, retrieves all embedded resources from HTML pages
        # retrieve-resources-regex: ^((?!google|facebook).)*$  # regular expression used to match any resource
                                                             # URLs found in HTML document against. Unset by default
        concurrent-pool-size: 4  # concurrent pool size for resources download, 4 by default
        use-dns-cache-mgr: true  # use DNS Cache Manager to test resources
                                 # behind dns load balancers. True by default.
        force-parent-sample: true  # generate only parent sample for transaction controllers.
                                   # True by default
        content-encoding: utf-8  # global content encoding, applied to all requests.
                                 # Unset by default
        follow-redirects: true  # follow redirects for all HTTP requests
        random-source-ip: false  # use one of host IPs to send requests, chosen randomly.
                                 # False by default
        requests:
        - url: '/api/v2/user/sign_in'
          method: POST
          headers:
            Content-Type: application/json
          body:
            email: "67webs@gmail.com"
            password: "123123123"

## Execution as root

    bzt test-sign_in.yaml


## References

https://gettaurus.org/

https://jtway.co/stress-testing-your-rails-application-using-jmeter-95a201071b98

https://flood.io/blog/how-to-test-a-ruby-on-rails-application-with-jmeter/

https://github.com/flood-io/ruby-jmeter

https://www.thoughtworks.com/insights/blog/performing-apache-jmeter-ruby-way

