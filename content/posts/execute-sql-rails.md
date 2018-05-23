+++
categories = ["rails"]
date = "2018-05-23T12:10:45+02:00"
description = ""
featured_image = ""
tags = ["sql"]
title = "Execute SQL query in Rails"

+++
Use ActiveRecord::Base.connection.exec_query to execute a SQL statement
<!--more-->

For example

    res=ActiveRecord::Base.connection.exec_query("select * from accounting_orchestrator_templates;")

