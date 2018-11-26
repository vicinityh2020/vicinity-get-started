In this section it is explained the structure of the web application. All the entities or building blocks that conform the app are presented individually and in relation to the others.

The backbone entities are 5, listed in order of importance below:

* Organisation: This is any company or association that own a smart energy facility and it is registered in the platform. An organisation can have many users and gateways underneath.

* User: These are actors, members of an organisation, that have a profile in the platform. Users can play different roles:
    * Device owner
    * Service provider
    * Infrastructure operator
    * Administrator
    * System integrator
    * User
* Gateway: It is an entity that represents a Gateway/Adapter. Adapters integrate a IoT infrastructure that contain one or more devices/services. In the platform, each adapter registers its own infrastructure.
* Item: It represents all devices and services.
* Contract: Every time an organisation requests to use a third party service for its devices, it is necessary to establish a contract between them. This entity stores the legal text, the ids of the parties involved and other relevant information to ensure a correct exchange of data.

To complement the social features of the platform there are other 2 entities for the notifications and the audit logs (History).

The relation between entities can be seen in the following picture:

.. image:: images/nm/img-architecture.png
   :align: center
