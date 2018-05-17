+++
date = "2018-05-17T16:41:07+02:00"
title = "Swagger-blocks response with JSON object"

+++
<!--more-->

      swagger_path '/supplier/services' do
        operation :get do
          key :description, 'List of users approved services'
          parameter do
            key :name, 'X-FLAVOUR'
            key :in, :header
            key :description, "Android App flavour. Possible values: 'client', 'supplier'"
            key :required, true
            key :type, :string
          end
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
    