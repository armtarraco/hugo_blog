+++
title = "Skip callbacks with RSpec testing"
date = 2018-05-18T23:50:23+02:00
featured_image = ""
description = ""
tags = [
  'callbacks'
]
categories = [
  'testing'
]
+++

<!--more-->

    class Address < ActiveRecord::Base
      after_create :create_in_odoo
    ...
    end


    FactoryGirl.define do
      factory :address do
        name    'address_name'

        after(:build) { |address| address.class.skip_callback(:create, :after, :some_method) }

      end
    end
