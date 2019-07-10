===========
Get started
===========

This "simple:)" get started guide provides step by step approach to integrate IoT infrastructure in VICINITY.

-----------------------------------------------
Install the VICINITY Gateway API from GitHub
-----------------------------------------------
We will start with simple VICINITY Gateway API installation.

1. Clone the github repository

  ::

    cd /path/to/the/directory
    git clone git@github.com:vicinityh2020/vicinity-gateway-api.git

2. Compile the VICINITY Gateway API using maven

  ::

    cd vicinity-gateway-api
    mvn clean package

3. Create dedicated system user for VICINITY Gateway API

  ::

    adduser --system --group --home /opt/ogwapi --shell /bin/sh ogwapi


4. Put it all in the right place

  ::

    mkdir /opt/ogwapi
    mkdir /opt/ogwapi/log
    mkdir /opt/ogwapi/config
    cp -r ./config /opt/ogwapi
    cp target/ogwapi-jar-with-dependencies.jar /opt/ogwapi/gateway.jar
    chown -R ogwapi:ogwapi /opt/ogwapi
    chmod u+x /opt/ogwapi/gateway.jar

5. Running VICINITY Gateway API.

  ::

    cd /opt/ogwapi
    su - ogwapi -c "nohup java -jar gateway.jar &"

6. Checking the running API
  In nohup.out you should see:

  ::

    CONFIG: HTTP Basic challenge authentication scheme configured.
    Sep 08, 2018 2:19:45 PM org.restlet.engine.connector.NetServerHelper start
    INFO: Starting the internal [HTTP/1.1] server on port 8181
    Sep 08, 2018 2:19:45 PM org.restlet.Application start
    INFO: Starting eu.bavenir.ogwapi.restapi.Api application
    Sep 08, 2018 2:19:45 PM org.restlet.engine.application.ApplicationHelper start
    FINE: By default, an application should be attached to a parent component in order to let application's outbound root handle calls properly.
    Sep 08, 2018 2:19:45 PM eu.bavenir.ogwapi.restapi.RestletThread run
    FINE: HTTP server component started.


-----------------------------------------------
Run the VICINITY Gateway API as a service
-----------------------------------------------
This procedure is for linux only.

1. You must first have the structure ready.

  ::
  
    .../ogwapi_folder/
    .../ogwapi_folder/ogwapi.jar
    .../ogwapi_folder/data/
    .../ogwapi_folder/log/
    .../ogwapi_folder/config/
    .../ogwapi_folder/config/GatewayConfig.xml

2. Create a file under **/etc/systemd/system/** with nano or vi and paste the example script below. 
   eg. **sudo vim /etc/systemd/system/ogwapi.service**
| 
3. Paste the code below in your new file:

  ::

    [Unit]
    Description = ogwapi service
    After = network.target

    [Service]
    Type = forking
    ExecStart = /usr/local/bin/ogwapi.sh start
    ExecStop = /usr/local/bin/ogwapi.sh stop
    ExecReload = /usr/local/bin/ogwapi.sh reload
    SuccessExitStatus=143
    Restart=always

    [Install]
    WantedBy=multi-user.target


4. Create a file with under **/usr/local/bin/** 
   (eg. **sudo vim /usr/local/bin/ogwapi.sh**) and put there the code below.
   

  ::

    #!/bin/sh 
    SERVICE_NAME=ogwapi 
    PATH_TO_JAR_FOLDER=/home/andrej/ogwapi 
    PATH_TO_JAR=$PATH_TO_JAR_FOLDER/ogwapi.jar 
    PID_PATH_NAME=/tmp/ogwapi-pid 
    cd $PATH_TO_JAR_FOLDER 
    case $1 in 
      start) 
          echo "Starting $SERVICE_NAME ..." 
          if [ ! -f $PID_PATH_NAME ]; then 
              nohup java -jar $PATH_TO_JAR >> $PATH_TO_JAR_FILE_FOLDER/ogwapiService.out 2>&1& 
              echo $! > $PID_PATH_NAME 
              echo "$SERVICE_NAME started ..." 
          else 
              echo "$SERVICE_NAME is already running ..." 
          fi 
      ;; 
      stop) 
          if [ -f $PID_PATH_NAME ]; then 
              PID=$(cat $PID_PATH_NAME); 
              echo "$SERVICE_NAME stoping ..." 
              kill $PID; 
              echo "$SERVICE_NAME stopped ..." 
              rm $PID_PATH_NAME 
          else 
              echo "$SERVICE_NAME is not running ..." 
          fi 
      ;; 
      restart) 
          if [ -f $PID_PATH_NAME ]; then 
              PID=$(cat $PID_PATH_NAME); 
              echo "$SERVICE_NAME stopping ..."; 
              kill $PID; 
              echo "$SERVICE_NAME stopped ..."; 
              rm $PID_PATH_NAME 
              echo "$SERVICE_NAME starting ..." 
              nohup java -jar $PATH_TO_JAR >> $PATH_TO_JAR_FILE_FOLDER/ogwapiService.out 2>&1& 
              echo $! > $PID_PATH_NAME 
              echo "$SERVICE_NAME started ..." 
          else 
              echo "$SERVICE_NAME is not running ..." 
          fi 
      ;; 
    esac

5. Modify the SERVICE_NAME, PATH_TO_JAR_FOLDER, and choose a PID_PATH_NAME for the file you are going to use to store your service ID.

6. Write the file and give execution permisions ex. **sudo chmod +x /usr/local/bin/ogwapi.sh**

7. Enable the service with the command **sudo systemctl enable ogwapi**

8. To run the service **sudo service ogwapi start**

9. To check the service status **sudo service ogwapi status**

10. To stop the service **sudo service ogwapi stop**

-----------------------------------------------
Install the VICINITY Example Adapter
-----------------------------------------------

We provide very simple Adapter example as a playground for first run and testing. The Adapter example is part of the VICINITY Agent installation.

Download prepared Adapter example from VICINITY Agent GitHub. In releases tab, find last release and download attached file **adapter-build-x.y.z.zip**, where **x.y.z** is the version of actual release. Unzip it.

Example Adapter exposes two **things**: *example-thing-1* and *example-thing-2*.
You can find their thing descriptions in file

::

    adapter-build-x.y.z/objects/example-objects.json


-----------------------------------------------
Install the VICINITY Agent Service
-----------------------------------------------

We provide very Agent Service build preconfigured to work with example Adapter.
Later, you can reconfigure of Agent Service to work with any other adapters just by changing the `Agent configuration file  <https://github.com/vicinityh2020/vicinity-agent/blob/master/docs/AGENT.md>`_

1.) Download prepared Agent Service from VICINITY Agent GitHub
  In releases tab, find last release and download attached file **agent-build-x.y.z.zip**, where **x.y.z** is the version of actual release. Unzip it.

2.) Register access point of VICINITY Neighbourhood Manager
  Agent needs to be authenticated to perform any registry operations in VICINITY Peer to Peer network. You can simple get credentials from VICINITY Neighbourhood Manager `Access point tab <https://github.com/vicinityh2020/vicinity-neighbourhood-manager/wiki/Access-points>`_

3.) Update Agent Service credentials in configuration file

  ::

     agent-build-x.y.z/config/agents/example-agent.json

  Fill in correct values in:

  ::

    "credentials": {
        "agent-id": "agent id goes here",
        "password": "agent password goes here"
    }

  Everything is now prepared for the first run.


-----------------------------------------------
Register your things
-----------------------------------------------

In the first run of complete VICINITY Node instalation, the things exposed by
example Adapter will be registered in VICINITY and they will obtain persistent
identifiers, under which they will be know to any other thing in VICINITY. The process
will go as follows:

* Agent Service will run the startup sequence, which includes discovery of objects exposed by adapters attached to Agent Service.

* New things will be registered into VICINITY, existing will be updated, missing deleted.

* Agent Service ends with actual configuration on VICINITY Node, all things are discovered,
online and available via Neighbourhood Manager.

Lets do this.

1.) Run VICINITY Gateway API** (see above)

2.) Run example Adapter**

  ::

      cd adapter-build-x.y.z
      ./adapter.sh

  Your Adapter is now running. In console, you should see:

  ::

      Oct 23, 2018 2:32:36 PM org.restlet.engine.connector.NetServerHelper start
      INFO: Starting the internal [HTTP/1.1] server on port 9998
      Oct 23, 2018 2:32:36 PM org.restlet.Application start
      INFO: Starting sk.intersoft.vicinity.adapter.testing.service.TestingAdapterApplication application
      starting

3.) Run Agent Service**

  ::

      cd agent-build-x.y.z
      ./agent.sh

  Your Agent service is now running. In console, you should see:

  ::

      command:
      pid:
      starting agent
      agent started

  Agent Service logs its whole process into file:

  ::

      agent-build-x.y.z/logs/agent-yyyy-mm-dd.log

  In few seconds, the startup sequence and discovery process should be completed.
  You can check your actual Agent Service configuration at endpoint


  ::

      GET http://localhost:9997/agent/configuration

  You can check it in your browser. You should see similar content

  ::

      {
        "adapters": [{
          "adapter-id": "example-adapter",
          "things": [
            {
              "adapter-infra-id": "example-adapter---!---example-thing-1",
              "infra-id": "example-thing-1",
              "password": "R1az6N72N7KfEvGYKVLp5f7PiS3Bv3prIfSkuyb0k+Y=",
              "agent-id": "f7f63ef6-fd8a-44f6-8a4a-c15f8376edaa",
              "adapter-id": "example-adapter",
              "oid": "f9d16d9e-02ec-40bc-ad38-4b814d62ea33",
              "adapter-oid": "example-adapter---!---f9d16d9e-02ec-40bc-ad38-4b814d62ea33"
            },
            {
              "adapter-infra-id": "example-adapter---!---example-thing-2",
              "infra-id": "example-thing-2",
              "password": "anea2CW6UAPikNfCYp+xZLsERIF0Mxys4hvZvRy9qNk=",
              "agent-id": "f7f63ef6-fd8a-44f6-8a4a-c15f8376edaa",
              "adapter-id": "example-adapter",
              "oid": "10c67501-9536-4b58-937a-804df9bdcde6",
              "adapter-oid": "example-adapter---!---10c67501-9536-4b58-937a-804df9bdcde6"
            }
          ],
          "subscribe-channels": [],
          "open-channels": []
        }],
    ...

  If you see configuration, discovery process was successfull and your example
  things were registered. Each thing obtained unique VICINITY **oid**. This is
  unique persistent identifier of your thing. Any other things in VICINITY can
  interact with other things using their VICINITY **oid**.

  Following the configuration above, our example things are mapped as follows:

  **example-thing-1**

  ::

      infrastructure-id: example-thing-1
      oid: f9d16d9e-02ec-40bc-ad38-4b814d62ea33


  **example-thing-2**

  ::

      infrastructure-id: example-thing-2
      oid: 10c67501-9536-4b58-937a-804df9bdcde6

  If you will run this step, you will receive unique specific **oid**s for your things.

  Now we are ready to interact with our example things.


-----------------------------------------------
Read data from your example thing
-----------------------------------------------

When your things were successfully registered, you need to enable them
in Neighbourhood Manager user interface. It is possible to interact only
with enabled things.

To simulate interaction between thing behind the adapter and another VICINITY thing,
we will use following Agent Service endpoint


::

    GET http://localhost:9997/agent/remote/objects/f9d16d9e-02ec-40bc-ad38-4b814d62ea33/properties/example-property
    headers:
    adapter-id=example-adapter
    infrastructure-id=example-thing-2

This call means, that thing inside **example-adapter** with its internal identifier **example-thing-2** wants
to read property of remote thing with VICINITY identifier **f9d16d9e-02ec-40bc-ad38-4b814d62ea33**.

Use Postman to perform this call. The response to this call will look as follows


::

    {
        "error": false,
        "statusCode": 200,
        "statusCodeReason": "OK",
        "message": [
            {
                "data": {
                    "echo": "get property",
                    "pid": "example-property",
                    "oid": "example-thing-1"
                },
                "status": "success"
            }
        ]
    }

Now you are officially integrated into VICINITY and you can interact with known things.

To correctly stop the Agent Service, run following command


::

    cd agent-build-x.y.z
    ./agent.sh stop
