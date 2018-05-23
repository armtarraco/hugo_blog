+++
categories = ["devops"]
date = "2018-05-11T09:40:14+02:00"
header = ["/uploads/2018/05/11/Experience.jpeg"]
tags = ["ssl"]
title = "Selfsigned SSL certificate for develpment"

+++
<!--more-->

[https://deliciousbrains.com/https-locally-without-browser-privacy-errors/](https://deliciousbrains.com/https-locally-without-browser-privacy-errors/ "https://deliciousbrains.com/https-locally-without-browser-privacy-errors/")

En la vagrant -> ip 192.168.50.50

Vagrantfile

    ...
      config.vm.define 'alfredo' do |alfredo|
        alfredo.vm.network "forwarded_port", guest: 3001, host: 3001, auto_correct: true
        alfredo.vm.network "private_network", ip: "192.168.50.50"
    ...
    

Generate keys

    openssl req -config openssl_selfsigned.conf -new -sha256 -newkey rsa:2048 -nodes -keyout server.key -x509 -days 365 -out server.crt

Fichero conf con

\[ req \]

default_bits        = 2048  
default_keyfile     = server-key.pem  
distinguished_name  = subject  
req_extensions      = req_ext  
x509_extensions     = x509_ext  
string_mask         = utf8only

\[ subject \]

countryName                 = Country Name (2 letter code)  
countryName_default         = US

stateOrProvinceName         = State or Province Name (full name)  
stateOrProvinceName_default = NY

localityName                = Locality Name (eg, city)  
localityName_default        = New York

organizationName            = Organization Name (eg, company)  
organizationName_default    = SHK

commonName                  = Common Name (e.g. server FQDN or YOUR name)  
commonName_default          = SHK

emailAddress                = Email Address  
emailAddress_default        = [test@example.com](mailto:test@example.com)

\[ x509_ext \]

subjectKeyIdentifier   = hash  
authorityKeyIdentifier = keyid,issuer

basicConstraints       = CA:FALSE  
keyUsage               = digitalSignature, keyEncipherment  
subjectAltName         = @alternate_names  
nsComment              = "OpenSSL Generated Certificate"

\[ req_ext \]

subjectKeyIdentifier = hash

basicConstraints     = CA:FALSE  
keyUsage             = digitalSignature, keyEncipherment  
subjectAltName       = @alternate_names  
nsComment            = "OpenSSL Generated Certificate"

\[ alternate_names \]

DNS.1       = dev-sharing-kitchen

---fin del fichero conf

  
The last section is called SAN SubjectAlternateNames

The important line is the last one: -> DNS.1

From the comments in the original post,  
For more rails apps in the same box, add DNS.2, DNS.3, ...

En el host

    /etc/hosts
    
    192.168.50.50 dev-sharing-kitchen

abrir [https://dev-sharing-kitchen:3001](https://dev-sharing-kitchen:3001/)

Developer Tools, Security, View certificate, in details tab, Export (to a file) -> SHK

Double-click in the certificate file, if it asks a password to unblock the "Gnome2 Key Storage" follow the next instructions.  
Gnome2 Key Storage used to be name "User Keys", so is a file

    "\~/.local/share/keyrings/user.keystore".

1\. Remove that file  
2\. Double-click on the certificate file to import it

A new 'user.keystore' will be created.  
Also, it will be appear a new file with the certificate -> SHK.cer

In Chrome, Advance Settings, Certificates

Importar servidor, abrir el archivo del certificado. Queda importado en la categoría de "Otros".

También es necesario habilitar

    chrome://flags/#allow-insecure-localhost

pero sigue mostrando el sitio como no seguro

Rails

    #config/environments/development.rb
    host = ENV['DEV_SHK_IP'] || '0.0.0.0'
    port = 3001
    asset_host = "https://#{host}:#{port}"
    config.action_mailer.asset_host = asset_host
    config.action_controller.asset_host = asset_host
    config.use_ssl = true
    config.ssl_port = port # start thin on port 3001

    #thin-vagrant.yml
    ---
    user: vagrant
    group: vagrant
    chdir: "/vagrant"
    environment: development
    address: 192.168.50.50
    port: 3001
    timeout: 30
    log: "log/thin.log"
    pid: "tmp/pids/thin.pid"
    max_conns: 1024
    max_persistent_conns: 100
    rackup: config.ru
    require: []
    wait: 30
    threadpool_size: 20
    daemonize: true
    servers: 1
    tag: shk
    ssl: true
    ssl-key-file: "/home/vagrant/.ssl/server.key"
    ssl-cert-file: "/home/vagrant/.ssl/server.crt"

Start the server with

    thin start -C thin-vagrant.yml

or

    thin start --ssl  --ssl-key-file /home/vagrant/.ssl/server.key --ssl-cert-file /home/vagrant/.ssl/server.crt -p 3001