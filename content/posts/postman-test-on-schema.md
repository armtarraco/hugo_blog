+++
title =  "Postman tests on JSON response schema"
date = 2018-09-20T09:21:35+02:00
featured_image = ""
description = ""
tags = []
categories = ["postman", "testing"]
+++

<!--more-->


If you want only true or false...


    pm.test('Schema is valid', function() {
      pm.expect(tv4.validate(responseJSON, schema)).to.be.true;
    });

then you get...

    FAIL Schema is valid | AssertionError: expected false to be true


But if you want reported some more information when it fails...

    pm.test('Schema is valid', function() {
      const valid = tv4.validate(responseJSON, schema);
      if (!valid) {
          pm.expect(tv4.error.message).to.eql("");
      } else {
          pm.expect(valid).to.be.true;
      }
    });

    pm.test('No missing keys in response', function() {
      const valid = tv4.validate(responseJSON, schema);
      if (!valid) {
          console.log("===missing keys=", tv4.missing);
      }
      pm.expect(tv4.missing).to.eql([]);
    });


then you get...


    FAIL Schema is valid | AssertionError: expected 'Invalid type: number (expected string)' to deeply equal ''

It is not a great information but it gives you a tip.

