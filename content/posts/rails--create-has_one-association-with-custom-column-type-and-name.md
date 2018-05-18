+++
date = "2018-05-02T09:15:24+02:00"
title = "Rails: create has_one association with custom column type and name"
tags = [
  "ruby",
]
categories = [
  "documentation",
]

+++
How to manage custom types like \`\`\`:uuid\`\`\`

<!--more-->

Migration:

    class AddSupplierToUser < ActiveRecord::Migration
      def change
        add_column :users, :supplier_id, :uuid
        add_index :users, :supplier_id, unique: true
        add_foreign_key :users, :providers, column: :supplier_id
      end
    end
    

Schema result

      create_table "users", id: :uuid, default: "uuid_generate_v4()", force: :cascade do |t|
        t.uuid     "supplier_id"
       ...
      end
      add_index "users", ["supplier_id"], name: "index_users_on_supplier_id", unique: true, using: :btree
    
      create_table "providers", id: :uuid, default: "uuid_generate_v4()", force: :cascade do |t|
    ...

In Postgres:

Indexes:

        "index_users_on_supplier_id" UNIQUE, btree (supplier_id)
        ...

Foreign Key constraints

        "fk_rails_d10566a884" FOREIGN KEY (supplier_id) REFERENCES providers(id)

Models:

    Class User < ActiverRecord::Base
      belongs_to  :supplier, class_name: 'Provider', dependent: :destroy, autosave: true
    ...
    Class Provider < ActiverRecord::Base
      has_one :user, autosave: true
    ...
