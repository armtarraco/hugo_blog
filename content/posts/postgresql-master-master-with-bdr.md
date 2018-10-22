+++
title =  "Postgresql Master Master With BDR"
date = 2018-10-22T09:18:35+02:00
featured_image = ""
description = ""
tags = []
categories = ['postgresql']
+++

# POSTGRESQL-BDR MASTER-MASTER INSTALLATION & CONFIGURATION

## Prepare Virtualbox boxes with Centos-7.5
Download minimal install CentOS7.5 iso from http://ftp.rediris.es/mirror/CentOS/

I used this one: http://ftp.rediris.es/mirror/CentOS/7.5.1804/isos/x86_64/CentOS-7-x86_64-Minimal-1804.iso

### Create first box

#### Recommended Network configuration
Adapter 1 -> NAT for accessing to the internet
Adapter 2 -> Only host for interconnecting boxes and accessing via ssh

#### Install CentOS-7
Add user deployer/password as admin (group wheel)

## NODE 1

### Console resolution
https://superuser.com/questions/816528/with-centos-7-as-a-virtualbox-guest-on-a-mac-host-how-can-i-change-the-screen-r/1133393

Add screen resolution in GRUB_CMDLINE_LINUX value in ```/etc/default/grub``` file
```/etc/default/grub```

    GRUB_CMDLINE_LINUX="crashkernel=auto ... rhgb quiet vga=0x0348"

Grub Update

    grub2-mkconfig -o /boot/grub2/grub.cfg

Reboot to check resolution changed to 1152x768

*Tip: vga=ask lets choose resolution on boot*

### Update system after installation
    sudo yum update
    sudo reboot now

### Install some tools
    sudo yum install -y mlocate
    sudo updatedb

### Prepare SSH access
    mkdir .ssh
    chmod 700 .ssh
    vi .ssh/authorized_keys

Paste deployer public key

    chmod 600 .ssh/authorized_keys

    /etc/ssh/sshd_config
    -> change port to SSH-PORT
    -> enable root login -> no

#### Firewall
    sudo firewall-cmd --permanent --add-port=SSH-PORT/tcp
    sudo systemctl restart sshd

try ssh login

    sudo visudo
    -> group wheel change to line NOPASSWD

### NTP time sync
    sudo yum install ntp
    sudo systemctl enable ntpd
    sudo systemctl start ntpd
    timedatectl

### Install BDR9.4 & PostrgeSQL 9.4

Install repo from https://dl.2ndquadrant.com/default/release/site/

    curl https://dl.2ndquadrant.com/default/release/get/bdr9.4/rpm | sudo bash
    sudo yum -y install epel-release  
    sudo yum -y install postgresql-bdr94-bdr

    sudo sed -i '/\[base\]/a exclude=postgresql*' /etc/yum.repos.d/CentOS-Base.repo
    sudo sed -i '/\[updates\]/a exclude=postgresql*' /etc/yum.repos.d/CentOS-Base.repo  

#### Initialize database

    locate initdb
    => /usr/pgsql-9.4/bin/initdb
    locate setup | grep postgre
    => /usr/pgsql-9.4/bin/postgresql94-setup

    sudo /usr/pgsql-9.4/bin/postgresql94-setup initdb
    => Initializing database ... OK

#### Configure PostgreSQL

    sudo su -
    locate postgresql.conf
    => /var/lib/pgsql/9.4-bdr/data/postgresql.conf
    vi /var/lib/pgsql/9.4-bdr/data/postgresql.conf

    listen_addresses = '*'
    port = 5432
    shared_preload_libraries = 'bdr'
    wal_level = 'logical'
    track_commit_timestamp = on
    max_connections = 100
    max_wal_senders = 10
    max_replication_slots = 10
    # Make sure there are enough background worker slots for BDR to run
    max_worker_processes = 10

    # These aren't required, but are useful for diagnosing problems
    log_error_verbosity = verbose
    log_min_messages = debug1
    log_line_prefix = 'd=%d p=%p a=%a%q '

    #------------------------------------------------------------------------------
    # BDR OPTIONS
    #------------------------------------------------------------------------------
    # Useful options for playing with conflicts
    bdr.default_apply_delay=2000   # milliseconds
    bdr.log_conflicts_to_table=on


#### Start and enable service

    systemctl start postgresql-9.4
    systemctl status postgresql-9.4
    => active running
    systemctl enable postgresql-9.4

#### Set password for user postgres

    sudo -u postgres psql postgres
    #postgres> \password postgres
    => enter pw   -> password

#### Permissions

    /var/lib/pgsl/9.4-bdr/data/pg_hba.conf

    # "local" is for Unix domain socket connections only
    local   all             all                                trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32       trust
    host    all             all             ip-node-1/32       trust
    host    all             all             ip-node-2/32       trust
    # IPv6 local connections:
    host    all             all             ::1/128            trust
    # Allow replication connections from localhost, by a user with the
    # replication privilege.
    #local   replication     postgres                          peer
    host    replication     postgres        127.0.0.1/32       trust
    host    replication     postgres        ip-node-1/32       trust
    host    replication     postgres        ip-node-2/32       trust
    host    replication     postgres        ::1/128            trust

As deployer

    vi ~/.pgpass
        #hostname:port:database:username:password
        localhost:5432:bdr-database:postgres:password
        localhost:5432:bdr-database:deployer:password
    sudo chmod 600 .pgpass

    sudo mkdir /home/postgres
    sudo cp /home/deployer/.pgpass /home/postgres/
    sudo chown postgres:postgres /home/postgres/
    sudo chown postgres:postgres /home/postgres/.pgpass

#### Firewall

    firewall-cmd --add-port=5432/tcp --permanent
    systemctl restart firewalld

#### Create database for testing configuration and install extensions

    createdb -U postgres bdr-database

    psql -U postgres bdr-database
    CREATE EXTENSION btree_gist;
    CREATE EXTENSION bdr;

#### Add node to bdr group

    SELECT bdr.bdr_group_create(
          local_node_name := 'node-1',
          node_external_dsn := 'port=5432 dbname=bdr-database password=password host=ip-node-1'
    );
    SELECT bdr.bdr_node_join_wait_for_ready();

#### Create database for testing configuration

At node-1

    CREATE TABLE t1bdr (c1 INT, PRIMARY KEY (c1));
    INSERT INTO t1bdr VALUES (1);
    INSERT INTO t1bdr VALUES (2);
    SELECT * FROM t1bdr;

At node-2
    DELETE FROM t1bdr WHERE c1 = 2;
    SELECT * FROM t1bdr;
    INSERT INTO t1bdr (c1) VALUES(6);
    SELECT * FROM t1bdr;

## NODE 2

**NOTE: Postgres database initialization must be done in each node**

#### Remove any existing postgresql database

    sudo rm -r /var/lib/pgsql/9.4-bdr/data

#### Initialize database

    sudo /usr/pgsql-9.4/bin/postgresql94-setup initdb

#### Same configuration than for node 1

    ~/.pgpass
    /var/lib/pgsl/9.4-bdr/data/pg_hba.conf
    /var/lib/pgsql/9.4-bdr/data/postgresql.conf 

    firewall-cmd --add-port=5432/tcp --permanent
    systemctl restart firewalld

#### Create same database for testing configuration and install extensions

    createdb -U postgres bdr-database

    psql -U postgres bdr-database
    CREATE EXTENSION btree_gist;
    CREATE EXTENSION bdr;

#### Add node to bdr group

    SELECT bdr.bdr_group_join( 
        local_node_name := 'node-2', 
        node_external_dsn := 'port=5432 dbname=bdr-database password=password host=ip-node-2',
        join_using_dsn := 'port=5432 dbname=bdr-database password=password host=ip-node-1'
    );

    SELECT bdr.bdr_node_join_wait_for_ready();

## Other BDR commands

    select * from bdr.bdr_nodes;
    select * from bdr.bdr_connections;

