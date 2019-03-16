.. note:: Linux machine. Tested in Debian Jessie and Ubuntu

If you are interested to deploy private instance of VICINITY Neighbourhood Manager please follow the following guideline.
In case you just want to make use of the platform please proceed to the next section of the wiki.

Pre-requisites
--------------

* Install GIT
* Install NodeJS 8.11.3
* Install Docker
* Install Mongo (Instructions below)
* [OPTIONAL] SSH certificates

Get the application
-----------------------

**From your home folder clone the repositories:**

::

  git clone https://jalmela@bitbucket.org/bavenir/vicinity_nm_ui.git
  git clone https://jalmela@bitbucket.org/bavenir/vicinity_nm_api.git

Run the client
--------------

**Install Bower**

Only first time

  ::

    sudo npm install -g bower

**Run Script**

Run the run.sh script with the configuration parameters that suits your infrastructure. See example and options below:

::

  run.sh -s -e dev -p 80 -b {web_url} -q 80 -a {api_url}

::

  -- Examples
  -- Production: ./run.sh -s -e prod -a my.server.com:PORT -b my.ui.com
  -- Development: ./run.sh -e dev -a localhost:3000 -b localhost:8080
  -- Local: ./run.sh [ without arguments, access on localhost:8080 ]
     -h  shows help
     -s  enables ssl [ Without arguments ]
     -e  environment [prod, dev, local (default)]
     -p  web port [8080 (default)]
     -q  api server port [3000 (default)]
     -b  web dns [localhost (default)]
     -a  api dns [localhost (default)]
     -w  workdir [ ~ (default)]
     -n  app name [nm-ui (default)]

Run the server
--------------

**Build your configuration file**

If you are authorized in the GPG chain of the project you can skip this step.

Create a file called vcnt_server.config.js in the same folder as the make.js script.

::

  touch ~/vicinity_nm_api/vcnt_server.config.js

Example of configuration:

::

  module.exports = {
    apps: [{
      name: "vcnt_server",
      script: "./server/bin/www",
      output: './logs/out.log',
      error: './logs/error.log',
      log: './logs/combined.outerr.log',
      merge_logs: true,
      instances: 0,
      watch: false,
      exec_mode: "cluster",
      env_development: {
        NODE_ENV: "development",
        env: "dev",
        PORT: 3000,
        SERVER_JWT_SECRET: ""
        SERVER_SSL_CERT: "/path/certificate/fullchain.pem",
        SERVER_SSL_KEY: "/path/certificate/privkey.pem",
        SERVER_MAX_PAYLOAD: "10mb",
        VCNT_MNGR_DB: "",
        vicinityServicesDir: "/path/additional_services/",
        COMMSERVER_TOKEN: "",
        COMMSERVER_URL: "",
        COMMSERVER_INSECURE: "",
        COMMSERVER_HOST: "",
        COMMSERVER_TIMEOUT_MS: "10000",
        SEMANTIC_REPO_URL: "",
        SEMANTIC_REPO_ADAPTERS:"",
        SEMANTIC_REPO_TIMEOUT_MS : "0",
        SMTP_HOST: "",
        SMTP_USER: "",
        SMTP_PASSWORD: "",
        SMTP_MAIL_SERVER: "",
        SMTP_APPROVER_MAIL: "",
        ELASTIC_APM_USE: "true",
        ELASTIC_APM_SERVICE_NAME: "",
        ELASTIC_APM_SERVER_URL: ""
      },
      env_production: {
        NODE_ENV: "production",
        env: "prod",
        PORT: 3000,
        SERVER_JWT_SECRET: ""
        SERVER_SSL_CERT: "/path/certificate/fullchain.pem",
        SERVER_SSL_KEY: "/path/certificate/privkey.pem",
        SERVER_MAX_PAYLOAD: "10mb",
        VCNT_MNGR_DB: "",
        vicinityServicesDir: "/path/additional_services/",
        COMMSERVER_TOKEN: "",
        COMMSERVER_URL: "",
        COMMSERVER_INSECURE: "",
        COMMSERVER_HOST: "",
        COMMSERVER_TIMEOUT_MS: "10000",
        SEMANTIC_REPO_URL: "",
        SEMANTIC_REPO_ADAPTERS:"",
        SEMANTIC_REPO_TIMEOUT_MS : "0",
        SMTP_HOST: "",
        SMTP_USER: "",
        SMTP_PASSWORD: "",
        SMTP_MAIL_SERVER: "",
        SMTP_APPROVER_MAIL: "",
        ELASTIC_APM_USE: "true",
        ELASTIC_APM_SERVICE_NAME: "",
        ELASTIC_APM_SERVER_URL: ""
      }
    }]
  }

Optional fields:

* Work without SSL connection: Skip SERVER_SSL_CERT and SERVER_SSL_KEY.
* Don not use a elastic search monitoring engine: ELASTIC_APM_USE: "false".

**Build the app**

::

  make.sh -e <env> -m <Your GPG chain mail [OPTIONAL]>

::

  Flags:
    -h  shows help
  Options with argument:
    -n  <app_name> [ vcnt-app (default) ]
    -e  <environment> [ development, production, local (default) ]
    -m  <git_secret_auth_mail> [ If missing, using local config ]
    -w  <path_where_repository_was_cloned> [ ~/vicinity_nm_api (default) ]


**Run the app**

You can update this script. Follow the instructions in run.sh if you want to make any changes.

Default running configuration is as follows:

::

  run.sh -d <Your domain>

::

  Flags:
      -h  shows help
  Options with argument:
      -n  <app_name> [ vcnt-app (default) ] [ OBLIGATORY ]
      -p  <port> [ 3000 (default) ] [ OBLIGATORY ]
      -d  <domain_name> [ OPTIONAL ]


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

Additional info
---------------

**Install Mongo with Docker**

You can use docker_compose to run the backend with an instance of Mongo. Docker_compose.yml example:

::

  version: "3.1"
  services:
    mongodb:
      networks:
      - dockernet
      image: mongo:latest
      container_name: mongodb
      restart: always
      environment:
      - MONGO_DATA_DIR=/data/db
      - MONGO_LOG_DIR=/data/log
      volumes:
      - ~/docker_data/db:/data/db
      - ~/docker_data/logs/mongo:/data/log/
      ports:
      - 27018:27018
      command: mongod --smallfiles --logpath /data/log/mongodb.log
    vas-monitor:
      networks:
      - dockernet
      volumes:
      - ~/docker_data/logs/vas-logs:/app/logs
      container_name: vcnt-app
      ports:
      - 3001:3001
      image: vcnt-app
      depends_on:
      - mongodb
  networks:
      dockernet:
          driver: bridge

You need to create the dockernet network

::

  docker network create -d bridge --subnet 192.168.0.0/24 --gateway 192.168.0.1 dockernet

**Install Mongo for debian**

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
