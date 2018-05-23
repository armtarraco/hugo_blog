+++
categories = ["testing"]
date = "2018-05-11T09:19:36+02:00"
tags = ["stubbing"]
title = "RSpec association stub"

+++
<!--more-->

    allow(service).to receive_message_chain(:user, :has_payment_method?) { true }
