+++
categories = ["testing"]
date = "2018-04-27T09:09:34+02:00"
tags = ["postman"]
title = "Postman tests sample"

+++
See more in Postman-echo collection in Postman App.

<!--more-->

    var responseJSON;
    
    try { 
        responseJSON = JSON.parse(responseBody); 
        tests['response is valid JSON'] = true;
    }
    catch (e) { 
        responseJSON = {}; 
        tests['response is valid JSON'] = false;
    }
    tests['response has post data'] = _.has(responseJSON, 'data');
    tests['response matches the data posted'] = (responseJSON.data && responseJSON.data.length === 256);
    tests["content-type equals text/plain"] = responseJSON && responseJSON.headers && (responseJSON.headers["content-type"] === 'text/plain');
    
    
    tests["Body contains headers"] = responseBody.has("headers");
    tests["Body contains args"] = responseBody.has("args");
    tests["Body contains url"] = responseBody.has("url");
    var responseJSON;
    try { responseJSON = JSON.parse(responseBody); }
    catch (e) { }
    tests["Args key contains argument passed as url parameter"] = 'test' in responseJSON.args
    
    
    tests["response code is 200"] = responseCode.code === 200;
    tests["response is sent in chunks"] = (postman.getResponseHeader('Transfer-Encoding') === 'chunked')
    
    var responseJSON;
    try {
        responseJSON = JSON.parse(responseBody); 
        tests["Status equals 200"] = responseJSON.status === 200;
    }
    catch (e) { }
    tests["Body contains status"] = responseBody.has("status");
    
    