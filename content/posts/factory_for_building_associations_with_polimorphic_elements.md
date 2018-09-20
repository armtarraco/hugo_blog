+++
title =  "Factory for building associations with polimorphic elements"
date = 2018-09-18T09:21:35+02:00
featured_image = ""
description = ""
tags = []
categories = []
draft = true
+++

class Service < ActiveRecord::Base
  has_many :ratings
  has_many :ratings_by_clients,   -> { where(rater_type: 'Client') }, class_name: 'Rating'
  has_many :ratings_by_suppliers, -> { where(rater_type: 'Provider') }, class_name: 'Rating'
end

class Rating < ActiveRecord::Base
  belongs_to :service
  belongs_to :rater, polymorphic: true

  scope :by_clients,   -> { where(rater_type: 'Client') }
  scope :by_suppliers, -> { where(rater_type: 'Provider') }
end

class User < ActiveRecord::Base
end

FactoryGirl.define do
  sequence :first_name do |n|
    "firstname-#{n}"
  end
end

FactoryGirl.define do
  sequence :email do |n|
    "name-#{n}@name.com"
  end
end

FactoryGirl.define do
  factory :supplier, class: User do
    email
    first_name
  end

  factory :customer, class: User do
    email
    first_name
  end
end

FactoryGirl.define do
  factory :rating do
    stars 3

    factory :rating_by_customer do
      association :rater, factory: :customer
      after(:create) do |rating|
        rating.rater_type = 'Client'
      end
    end

    factory :rating_by_supplier do
      association :rater, factory: :supplier
      after(:create) do |rating|
        rating.rater_type = 'Provider'
      end
    end
  end
end

