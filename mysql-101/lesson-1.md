# Installation and basic usage

## Goal

  - Installing on Linux (debian, fedora)
  - Knowing the data directory
  - Basic Administration: start/stop, reset password, privileges, secure installation
  - Connecting to the database
  - Basic Usage: create/query/drop databases and tables
  - Basic configuration


## Course setup

Install the following packages via yum or apt-get

    tree dstat vim hostname
    
Download all of the MySQL packages via yum or from MySQL website.

Unpack the Employee and Sakila databases

        bash#wget http://bit.ly/1HMHCBf    
        bash#tar xf employee*


## Installing MySQL
### Debian & derivates

    #dpkg -i mysql*deb

### Fedora & derivates

    yum -y install mysql-repo.rpm
    yum -y install MySQL-*
    yum -y install mysql-utilities




## Installing MySQL - 1

Check installed files
  
        #dpkg -L mysql-*
        #id mysql
        #find /etc/ -name \*mysql\*

Programs

        client                  server
        mysql                   mysqld
        mysql_config_editor     mysqld_safe
        mysqladmin              mysql_install_db
        mysqlimport
        mysqldump
        mysqlbinlog
        
## Installing MySQL
First access
          
        #cat /root/.mysql_secret
        #mysql -u$USER -p$PASSWORD [ -h$HOST -P$PORT ]
        #mysql -e "$QUERY"
  
## Installing MySQL - 2
On Ubuntu/Debian
        
        # cd /opt/mysql/server-5.6/
        # cp support-files/mysql.server /etc/init.d/mysql
        # cp my.cnf /etc/my.cnf
        # useradd mysql -s /usr/sbin/nologin -d /var/lib/mysql
        # chown -R mysql:mysql /var/lib/mysql
        # cat > /etc/profile.d/mysql.sh <<'EOF'
        export MYSQL_HOME=/opt/mysql/server-5.6/
        export PATH+=:$MYSQL_HOME/bin:$MYSQL_HOME/scripts
        EOF
        
        
## Installing MySQL - 3
Prepare a minimal configuration file...

        # /etc/my.cnf
        [mysqld]
        ...mysql defaults...
        # System user for mysql
        #  create if not exists
        user=mysql
        
        # All data files will go
        #  there
        datadir=/var/lib/mysql
        

## The datadir
Create or recreate the default one (from my.cnf)
  
        #mysql_install_db 

Or create alternative ones
  
        #mysql_install_db --datadir=/data2
        

## The datadir
Content of 

    /var/lib/mysql/
    |-- [mysql    mysql      19]  foo/
    |-- [mysql    mysql    4.0K]  mysql/
    |-- [mysql    mysql    4.0K]  performance_schema/
    |-- [mysql    mysql       3]  a02f12e917b1.pid
    |-- [mysql    mysql      56]  auto.cnf
    |-- [mysql    mysql     48M]  ib_logfile0
    |-- [mysql    mysql     48M]  ib_logfile1
    |-- [mysql    mysql     12M]  ibdata1
    `-- [mysql    mysql       0]  mysql.sock

     
  - application logs
  - DDL definitions .frm and indexes
  - InnoDB log files 
  - InnoDB tablespace
  - Binary & Relay Logs (*next lessons)


        
## Installing MySQL - 4
Further parameters

        # /etc/my.cnf
        [mysqld]
        ...mysql defaults...
        port=13306
        socket=/data2/mysql.sock
        pid-file=/data2/hostname.pid
        tmpdir=/tmp/data2
        log=/data2/hostname.log
  
# MySQL Service    
## The MySQL Service
Manage as a service
  
        service mysql [start|stop|status]
        
Run standalone

        mysqld [$PARAMETERS]
        
Or wrapped with a restart-daemon
        
        mysqld_safe        


## The MySQL Service
Check the server status at various levels with
     
     mysqladmin ping
     mysqladmin status
     mysqladmin extended-status
     mysqladmin processlist


## The MySQL Service
Stop mysql via

        #mysqladmin shutdown

Or with kill -TERM

        #kill -15 $MYSQL_PID

*NEVER kill -9*: this will corrupt your database!

## Connecting 
Client programs:

- mysql, mysqladmin, mysqlbackup

- full help

        #mysql ... [--help [--verbose]] 
        
- connect via socket or forcing TCP

        #mysql --socket=/var/lib/mysql/mysql.sock
        #mysql --protocol tcp
        
        SHOW DATABASES;
