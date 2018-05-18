+++
date = "2018-05-17T16:41:07+02:00"
title = "Swagger-blocks response with JSON object"
tags = [
  "swagger",
  "ruby",
]
categories = [
  "documentation",
]

+++
<!--more-->

In Rails controller

    swagger_path '/supplier/services' do
      operation :get do
        key :description, 'List of users approved services'
        response 200 do
          key :description, 'List of approved services'
          schema do
            key :type, :object
            property :services do
              key :type, :array
              items do
                key :'$ref', :ServiceInfoResponseForSupplierModel
              end
            end
          end
        end
      end
    end

Generates this in SwaggerHub

    {
      "services": [
        {
          "id": "string",
          "status": "string",
          "start_date": "string",
          "end_date": "string",
          "done_date": "string",
          "where": "string",
          "price": 0,
          "tax_amount": 0,
          "products": [
            {
              "id": "string",
              "name": "string",
              "service_id": "string",
              "unity_price": 0,
              "quantity": 0,
              "subtotal": 0,
              "tax_amount": 0,
              "eelp_fee": 0
            }
          ],
          "client_image": "string",
          "client_full_name": "string",
          "additional_information": "string"
        }
      ]
    }

Live example:

    {
        "services": [
            {
                "id": "SO03331",
                "status": "accepted",
                "start_date": "2018-12-29T14:53:34.000Z",
                "end_date": "2018-12-29T14:53:34.000Z",
                "done_date": null,
                "where": "Amargura, 323\nEdificio Azul\n08082 Barcelona\nEspaña",
                "price": 0,
                "tax_amount": null,
                "products": [
                    {
                        "id": 3881,
                        "name": "Abastecimiento para embarcaciones",
                        "service_id": "SO03331",
                        "unity_price": null,
                        "quantity": "1.0",
                        "subtotal": 0,
                        "tax_amount": null,
                        "eelp_fee": null
                    }
                ],
                "client_image": "https://s3-eu-west-1.amazonaws.com/eelp/images/users/cfcdd8f2-4b8e-4dba-90fd-a33c841f2d78/user_pics/1519314481_file.jpeg",
                "client_full_name": "Alfredoqa Demástu",
                "additional_information": null
            },
            {
                "id": "SO03334",
                "status": "accepted",
                "start_date": "2018-12-29T14:53:34.000Z",
                "end_date": "2018-12-29T14:53:34.000Z",
                "done_date": null,
                "where": "Amargura, 323\nEdificio Azul\n08082 Barcelona\nEspaña",
                "price": 40,
                "tax_amount": null,
                "products": [
                    {
                        "id": 3887,
                        "name": "Abastecimiento para embarcaciones",
                        "service_id": "SO03334",
                        "unity_price": "40.0",
                        "quantity": "1.0",
                        "subtotal": 40,
                        "tax_amount": null,
                        "eelp_fee": null
                    }
                ],
                "client_image": "https://s3-eu-west-1.amazonaws.com/eelp/images/users/cfcdd8f2-4b8e-4dba-90fd-a33c841f2d78/user_pics/1519314481_file.jpeg",
                "client_full_name": "Alfredoqa Demástu",
                "additional_information": null
            },
    ...