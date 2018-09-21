+++
title = "Factories with initialization params"
date = 2018-09-21T09:21:35+02:00
featured_image = ""
description = ""
tags = []
categories = ["testing"]
+++

<!--more-->

With a class like...

    class User < ActiveRecord::Base

      def initialize(attrs={})
        is_provider = attrs.fetch(:is_provider) { false }
        provider_params_for_register = attrs.delete(:provider_params_for_register)
        super

        is_provider ? create_supplier(provider_params_for_register) : create_client
        self
      end

    end


A posible factory is...

    FactoryGirl.define do
      factory :user do
        email
        password   '12345678'
        name       'provider'

        initialize_with { new(attributes.merge(is_provider: true, provider_params_for_register: { supplier_type: 0, occupations: [] })) }

      end
    end

