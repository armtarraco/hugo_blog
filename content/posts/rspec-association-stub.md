+++
categories = ["testing"]
date = "2018-05-11T09:19:36+02:00"
header = []
tags = ["stubbing"]
title = "RSpec association stub"
type = "basic_with_header_image"

+++
<!--more-->

    allow(service).to receive_message_chain(:user, :has_payment_method?) { true }