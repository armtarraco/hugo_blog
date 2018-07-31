# README

Blog done with Hugo.

More info at https://gohugo.io

# Deploy with rsync

More info at: https://gohugo.io/hosting-and-deployment/deployment-with-rsync/

Destinaiton url in file `conf.toml`

## Compile with 

    hugo

## Publish with rsync

    rsync -avz -e "ssh -p $portNumber" --delete public/ remote_server:/var/www/remote_folder

## Oneliner

    hugo && rsync -avz -e "ssh -p $portNumber" --delete public/ remote_server:/var/www/remote_folder
