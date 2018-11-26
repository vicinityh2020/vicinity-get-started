.. note:: Linux machine. Tested in Debian Jessie and Ubuntu

If you are interested to deploy private instance of VICINITY Neighbourhood Manager please follow the following guideline.
In case you just want to make use of the platform please proceed to the next section of the wiki.

Pre-requisites
--------------

* GIT: https://git-scm.com/
* NodeJS: http://nodejs.org/

Get the web application
-----------------------

**Create and navigate to the folder where the repository should be cloned:**

  ::

    mkdir /var/www/html
    cd /var/www/html

**Clone the repository and rename the folder:**

  ::

    git clone https://jalmela@bitbucket.org/bavenir/vicinity.git

**Point the webApp to the API URL:**

  ::

    cd /var/www/html/vicinity/vicinityManager/client/app
    sudo vim env.js

* Comment all the lines except the first
* Add at the bottom: this.env.apiUrl = "http://<IP>:<PORT>"

Install and configure Mongo DB
------------------------------

**Install**

1. Import they key for the official MongoDB repository.

  ::

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

2. After successfully importing the key, you will see:

  ::

    Output
    gpg: Total number processed: 1
    gpg:               imported: 1  (RSA: 1)

3. Next, we have to add the MongoDB repository details so apt will know where to download the packages from. Issue the following command to create a list file for MongoDB.

    * WARNING: It may change depending on the Linux distribution
    * echo "deb http://repo.mongodb.org/apt/debian jessie/mongodb-org/3.4 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

4. After adding the repository details, update the packages list and install the MongoDB package.

  ::

    sudo apt-get update
    sudo apt-get install -y mongodb-org

5. Create target folders for mongo and give permissions to the mongo user on that folders.

  ::

    mkdir /data/db
    chown -R mongodb:mongodb /data/db
    chown -R mongodb:mongodb /var/lib/mongodb

6.  Set paths in configuration file (/etc/mongod.conf) to:

    * Storage --> dbPath: /data/db
    * SystemLog --> path: /var/log/mongodb/mongod.log
  .. note:: See at the end how to add users to MONGO if you want to use authorization for security
  .. note:: BE CAREFUL WITH THE SPACES -- YAML FORMAT

7. Once MongoDB installs, start the service, and ensure it starts when your server reboots:

  ::

    sudo systemctl enable mongod.service
    sudo systemctl start mongod
    sudo systemctl status mongod  (Check it runs properly)

Run the client
--------------

**Install Angular Web App**

  ::

    sudo npm install -g bower
    cd /var/www/vicinity/vicinityManager/client
    sudo bower install --allow-root
    sudo npm install

**Install Apache**

  ::

    sudo apt-get install apache2

**Configure apache**

  ::

    cd /etc/apache2/sites-available
    sudo touch [server-name].conf

* Create conf:

  ::

    <VirtualHost *:80>
            ServerName [server name]
            ServerAdmin [admin mail]
            DocumentRoot [location of your index file]
            ErrorLog ${APACHE_LOG_DIR}/error.log
            CustomLog ${APACHE_LOG_DIR}/access.log combined
            Redirect permanent / [server name]
    </VirtualHost>
    <IfModule mod_ssl.c>
            <VirtualHost _default_:443>
                    ServerAdmin [admin mail]
                    DocumentRoot [location of your index file]
                    ErrorLog ${APACHE_LOG_DIR}/error.log
                    CustomLog ${APACHE_LOG_DIR}/access.log combined
                    SSLEngine on
                    #   A self-signed (snakeoil) certificate can be created by installing
                    #   the ssl-cert package. See
                    #   /usr/share/doc/apache2/README.Debian.gz for more info.
                    #   If both key and certificate are stored in the same file, only the
                    #   SSLCertificateFile directive is needed.
                    SSLCertificateFile      [ cert file ]
                    SSLCertificateKeyFile   [ key file ]
                    <FilesMatch "\.(cgi|shtml|phtml|php)$">
                                    SSLOptions +StdEnvVars
                    </FilesMatch>
                    <Directory /usr/lib/cgi-bin>
                                    SSLOptions +StdEnvVars
                    </Directory>
                    BrowserMatch "MSIE [2-6]" \
                                    nokeepalive ssl-unclean-shutdown \
                                    downgrade-1.0 force-response-1.0
                    # MSIE 7 and newer should be able to use keepalive
                    BrowserMatch "MSIE [17-9]" ssl-unclean-shutdown
            </VirtualHost>
    </IfModule>

* Save your conf file
* Disable default site

  ::

    sudo a2dissite 000-default.conf

* Enable your site

  ::

    sudo a2ensite [your server].conf

* Enable SSL
  ::

    sudo a2enmod ssl

* Start the service

  ::

    sudo service apache2 start

Run the server
--------------

**Install Node JS app**

  ::

    cd /var/www/vicinity/vicinityManager/server
    sudo npm install

**Install forever**

  ::

    npm install –g forever
    sudo mkdir /var/run/forever

**Create a service**

  ::

    sudo touch /etc/init.d/vcnt_server
    sudo chmod a+x /etc/init.d/vcnt_server
    sudo update-rc.d vcnt_server defaults

**Script**

* Update environmental variables within the script below. Use the corrensponding information for your deployment.

  ::

    #!/bin/bash
    ### BEGIN INIT INFO
    # If you wish the Daemon to be lauched at boot / stopped at shutdown :
    #
    #    On Debian-based distributions:
    #      INSTALL : update-rc.d scriptname defaults
    #      (UNINSTALL : update-rc.d -f  scriptname remove)
    #
    #    On RedHat-based distributions (CentOS, OpenSUSE...):
    #      INSTALL : chkconfig --level 35 scriptname on
    #      (UNINSTALL : chkconfig --level 35 scriptname off)
    #
    # chkconfig:         2345 90 60
    # Provides:          /var/www/html/vicinity/vicinityManager/server/bin/www
    # Required-Start:    $remote_fs $syslog
    # Required-Stop:     $remote_fs $syslog
    # Default-Start:     2 3 4 5
    # Default-Stop:      0 1 6
    # Short-Description: forever running /var/www/vicinity/vicinityManager/server/bin/www
    # Description:       /var/www/html/vicinity/vicinityManager/server/bin/www
    ### END INIT INFO
    #
    # initd a node app
    # Based on a script posted by https://gist.github.com/jinze at https://gist.github.com/3748766
    #
    if [ -e /lib/lsb/init-functions ]; then
    	# LSB source function library.
    	. /lib/lsb/init-functions
    fi;
    pidFile="/var/log/vicinity/vcnt_server.pid"
    logFile="/var/log/vicinity/vcnt_server.log"
    outFile="/var/log/vicinity/vcnt_server.out"
    errFile="/var/log/vicinity/vcnt_server.err"
    command="node"
    nodeApp="/var/www/html/vicinity/vicinityManager/server/bin/www"
    foreverApp="forever"
    ### EXPORT environmental variables
    # PORT
    export PORT={port}
    # MONGO DB URL
    # export VCNT_MNGR_DB="mongodb://[name]:[pwd]@[IP or localhost]:[port]/vicinity_neighbourhood_manager
    export VCNT_MNGR_DB="mongodb://localhost:27017/vicinity_neighbourhood_manager"
    # Comm Server URL and token
    export commServerToken={user:password}
    export commServerUrl={url}
    # JWT Token secret
    export jwtTokenSecret={secret key}
    # Semantic Repository URL
    export semanticRepoUrl={url}
    export enabledAdapters={semantic repo adapters}
    # SMTP configuration
    export smtpHost={host}
    export smtpUser={mail}
    export smtpPassword={password}
    export mailServer={mail}
    export approverMail={mail}
    # Certificates
    export cert={certificate}
    export key={private key}
    start() {
       echo "Starting $nodeApp"
       # Notice that we change the PATH because on reboot
       # the PATH does not include the path to node.
       # Launching forever with a full path
       # does not work unless we set the PATH.
       PATH=/usr/local/bin:$PATH
    	export NODE_ENV=production
       PORT=$PORT VCNT_MNGR_DB=$VCNT_MNGR_DB $foreverApp start --pidFile $pidFile -l $logFile -o $outFile -e $errFile -a -d -c "$command" $nodeApp
       RETVAL=$?
    }
    restart() {
    	echo -n "Restarting $nodeApp"
    	$foreverApp restart $nodeApp
    	RETVAL=$?
    }
    stop() {
    	echo -n "Shutting down $nodeApp"
       $foreverApp stop $nodeApp
       RETVAL=$?
    }
    status() {
       echo -n "Status $nodeApp"
       $foreverApp list
       RETVAL=$?
    }
    case "$1" in
       start)
            start
            ;;
        stop)
            stop
            ;;
       status)
            status
           ;;
       restart)
       	restart
            ;;
    	*)
           echo "Usage:  {start|stop|status|restart}"
           exit 1
            ;;
    esac
    exit $RETVAL

**Run service**

  ::

    sudo mkdir /var/log/vicinity
    sudo touch /var/log/vicinity/vcnt_server.out
    sudo touch /var/log/vicinity/vcnt_server.log
    sudo touch /var/log/vicinity/vcnt_server.pid
    sudo touch /var/log/vicinity/vcnt_server.err
    sudo service vcnt_server start

Putting all together -- First user and organisation in the app
--------------------------------------------------------------
**To start using the web app we need to create the first user manually**

* Basic set up

    * Create dB vicinity_neighbourhood_manager in Mongo
    * Create the collections user and useraccounts

* Insert first organisation in Mongo – In the useraccount collection

  ::

    db. useraccounts.insert({
        "name" : "admin",
        "cid" : "admin",
        "businessId" : "00000000",
        "skinColor" : "black",
        "location" : "test",
        "status" : "active"
    })

* Find organisation and copy the Mongo Id

    * db.useraccounts.find({organisation: organisationName}).pretty()

* Insert first user – In the user collection

  ::

      db.users.insert({
        "email" : "admin@admin.com",
        "occupation" : "admin",
        "name" : "admin",
        "location" : "test",
        "authentication" : {
            "hash" : REQUEST FIRST PASSWORD TO BAVENIR,
            "principalRoles" : [
                "user",
                "devOps",
                "administrator"
            ]
        },
        "accessLevel" : 1,
        "cid" : {
            "id" : < organisation MongoId >,
            "extid" : "admin"
        },
        "status" : "active"
      }
    })

* Add your new user to the organisation

    * db.useraccounts.update({'organisation': organisationName},{'accountOf': { $push:{'id': < user MongoId >, 'extid': "admin@admin.com" }}})

* Try to log in

    * Navigate your browser to the app domain and use the mail and password to do the first log in.

Old
---

**Server configuration using NGINX**

1. Install NGINX -- https://www.nginx.com/resources/wiki/start/

  ::

      sudo apt-get update
      sudo apt-get upgrade
      sudo apt-get install nginx

2. Configure NGINX

    * https://www.linode.com/docs/web-servers/nginx/how-to-configure-nginx

3. Backup the old configuration

  ::

    cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup
    sudo service nginx reload

4. Create server files and paste the script below

  ::

      sudo touch /etc/nginx/sites-available/vicinity
      sudo touch /etc/nginx/sites-enabled/vicinity

5. Script vicinity

  ::

    server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/vicinity/vicinityManager/client/app;

    # Add index.php to the list if you are using PHP
    index index.html index.htm index.nginx-debian.html;

    server_name _;

      location / {
      # First attempt to serve request as file, then
      # as directory, then fall back to displaying a 404.
      try_files $uri $uri/ =404;
      }
    }

6. Run the service

  ::

    sudo rm /etc/nginx/sites-enabled/default
    sudo service nginx restart

OPTIONAL
--------

**Add authentication to MONGO**

1. Create admin user

    * mongo
    * Now in mongo CLI...

    ::

      > use admin

    * Create admin ...

    ::

      > db.createUser({
         user: "name",
         pwd: "pwd",
         roles : [{
              role : "userAdminAnyDatabase",
              db : "admin"
         }]
      })

    * Close the CLI...

    ::

      > quit()

    * mongo -u [user] -p [pwd] --authenticationDatabase "admin"
    * Again in the CLI

    ::

      > use vicinity_neighbourhood_manager

    * Create user ...

    ::

        > db.createUser({
           user: "name",
           pwd: "pwd",
           roles : [{
                role : "readWrite",
                db : "vicinity_neighbourhood_manager"
             }]
          })

    * Close CLI
    * vim /etc/mongod.conf
    * Add or uncomment

    ::

      security
        authorization: 'enabled'

    * Save file
    * sudo service mongod restart
    * Remember to update /etc/init.d/vcnt_server with the new MONGO connection string with authentication. Just uncomment and complete the URL that has user and password
