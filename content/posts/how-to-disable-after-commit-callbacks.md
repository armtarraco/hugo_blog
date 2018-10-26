+++
title =  "How to Disable After Commit Callbacks"
date = 2018-10-26T09:08:57+02:00
featured_image = ""
description = ""
tags = []
categories = []
+++

Add to the MODEL_NAME FactoryGirl factory

    after(:build) { |MODEL_NAME| MODEL_NAME.class.skip_callback(:commit, :after, :CALLBACK_METHOD) }

to avoid calling the ```after_create :CALLBACK_METHOD```
