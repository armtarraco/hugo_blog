+++
title =  "How to Connect to Odoo"
date = 2018-07-19T08:16:42+02:00
featured_image = ""
description = ""
tags = []
categories = ['odoo', 'rails']
+++

DRAFT

NOT TESTED

<!--more-->

    #config/odoo_raw_connection.yml
    staging:
      host: odoo.server.com
      dbname: odoo_database
      user: odoo_user
      password: odoo_password



    config = Rails.application.config_for(:raw_odoo_connection)
    conn = PG.connect(config)
    conn.exec("Select * from product_public_category_product_template_rel;")
