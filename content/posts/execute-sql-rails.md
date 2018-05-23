+++
title = "Execute SQL query in Rails"
date = 2018-05-23T12:10:45+02:00
featured_image = ""
description = ""
tags = ["sql"]
categories = ["rails"]
+++
Use ActiveRecord::Base.connection.exec_query to execute a SQL statement
<!--more-->

For example

    res=ActiveRecord::Base.connection.exec_query("select * from accounting_orchestrator_templates;")

