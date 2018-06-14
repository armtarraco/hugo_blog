+++
title =  "Change column type with manual casting"
date = 2018-06-14T13:01:13+02:00
featured_image = ""
description = ""
tags = []
categories = ['rails']
+++

The following migration throws an exception when trying to change the 'stars' column type from string to decimal.

    class ChangeColumnStarsOfRating < ActiveRecord::Migration
      def change
        change_column :ratings, :stars, :decimal, precision: 3, scale: 1
      end
    end

The error is:

    == 20180614104639 ChangeColumnStarsOfRating: migrating ========================
    -- change_column(:ratings, :stars, :decimal, {:precision=>3, :scale=>1})
    rake aborted!
    StandardError: An error has occurred, this and all later migrations canceled:

    PG::DatatypeMismatch: ERROR:  column "stars" cannot be cast automatically to type numeric

The solution with a reversible migration is

    class ChangeColumnStarsOfRating < ActiveRecord::Migration
      def self.up
        change_column :ratings, :stars, 'decimal USING stars::numeric(3,1)'
      end
      
      def self.down
        change_column :ratings, :stars, :string
      end
    end


Another possibility is using the Postgres function CAST

    change_column :ratings, :stars, 'decimal USING CAST(stars AS integer)'
