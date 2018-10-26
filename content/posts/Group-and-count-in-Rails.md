+++
title =  "Group and Count in Rails"
date = 2018-10-26T09:47:11+02:00
featured_image = ""
description = ""
tags = []
categories = ['rails']
+++

With ActiveRecord query

    User.order("TO_CHAR(created_at, 'YYYY MM')").group("TO_CHAR(created_at, 'YYYY MM')").count


With plain SQL query

    sql = "Select * from ... your sql query here"
    records_array = ActiveRecord::Base.connection.execute(sql)
