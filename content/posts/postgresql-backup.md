+++
title =  "Postgresql Backup"
date = 2018-10-26T10:11:04+02:00
featured_image = ""
description = ""
tags = []
categories = ['postgresql']
+++


# Backup

    pg_dump -Fp -U postgres eelp-backend | gzip > /tmp/backup/eelp-backend-backup.sql.gz

or

    pg_dump -U {user-name} {source_db} -f {dumpfilename.sql} --data-only


# Restore

    psql [--table {table-name}] -U {user-name} -d {destination_db}-f {dumpfilename.sql}


# Rails load schema for new developers instead of execute all migrations

    RAILS_ENV=development bundle exec rake db:drop
    RAILS_ENV=development bundle exec rake db:create
    RAILS_ENV=development bundle exec rake db:schema:load

And restore database
