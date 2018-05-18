+++
categories = []
date = "2018-05-19T00:33:33+02:00"
description = ""
featured_image = ""
tags = []
title = "RSpec association stubbing"

+++
<!--more-->

    allow(service).to receive_message_chain(:user, :has_payment_method?) { true }