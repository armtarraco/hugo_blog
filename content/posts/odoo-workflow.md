+++
date = "2018-05-15T09:29:46+02:00"
draft = true
title = "Odoo workflow"

+++
<!--more-->

[https://stackoverflow.com/a/43648668](https://stackoverflow.com/a/43648668 "https://stackoverflow.com/a/43648668")

  
I want to confirm sales order as soon as it gets created.  
...  
You should use the method action_confirm:

    so = models.execute_kw(db, uid, password,  'sale.order', 'search',   [[['name', '=', 'SO004']]])print soprint models.execute_kw(db, uid, password, 'sale.order', 'action_confirm', so)