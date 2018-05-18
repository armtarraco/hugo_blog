+++
date = "2018-05-15T09:31:50+02:00"
title = "Odoo workflow for SaleOrder"
tags = [
  "odoo",
]
categories = [
  "rails",
]

+++
<!--more-->

    so=SaleOrder.find(3468)
    so.wkf_action('quotation_sent')
    so.state
    => sent
    so.wkf_action('order_confirm')
    so.state
    => manual
    so.wkf_action('manual_invoice')
    so.state
    => progress

    inv=so.invoice_ids[0]
    inv.state
    => draft
    inv.wkf_action('invoice_open')
    inv.state
    => open

    voucher = AccountVoucher.new({:amount=>inv.amount_total, :type=>"receipt", :partner_id => inv.partner_id.id}, {"default_amount"=>inv.amount_total, "invoice_id"=>inv.id})

    #def on_change(on_change_method, field_name, field_value, *args)
    #el primer 1 es el id del AccountJournal de ventas
    voucher.on_change("onchange_partner_id", [], :partner_id, inv.partner_id.id, 1, inv.amount_total, 1, 'receipt', false)
    voucher.save
    => draft
    voucher.wkf_action('proforma_voucher')
    => posted
