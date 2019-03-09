This section is about the Access Points, which are the virtual
representation of an infrastructure. In a real scenario, an access point
represents 2 or 3 components:

* Gateway: Software that is used to communicate with the platform's communication server and the P2P network. See all information regarding the VICINITY Open Gateway `HERE <../open-gateway-api.rst>`_
* Agent: [OPTIONAL] It is used to register the smart components of an infrastructure into the platform. It implements logic to handle the process of registering, updating and removing items from/to the platform. It needs to have the infrastructure components described in a common format. See all information regarding the VICINITY agent `HERE <https://github.com/vicinityh2020/vicinity-agent>`_
* Adapter: It is the specific software that translates the infrastructure conventions, definitions or API endpoints into the common format. It is mapping all the properties and capabilities of each smart component so the platform can understand what it is. It could implement some of the agent functionalities depending on the needs.

To be able to manage the access points in the platform, a user needs to have the system integrator role. Having this role opens the access point view in the sidebar,

.. image:: images/nm/img-gateways.png
   :align: center
   :width: 300px
   :height: 600px

and from this view it is possible to manage all the access points. The image below is going to be referenced in the following sub-sections to describe all its details.

.. figure:: images/nm/img-gat-view.png
   :align: center

   Example figure

Properties
##########

As we can see in the example figure there are 3 main properties.

======== ===================================================== ======
Property Description                                           Update
======== ===================================================== ======
Name     Friendly name                                         YES
AGID\*   External ID                                           NO
Type     Indicates to which project belongs the infrastructure YES
======== ===================================================== ======

.. note:: This is an important field because when initialising the agent/adapter, this ID will be used in the authentication credentials together with the password. The password will be introduced by the system integrator when creating the gateway entity and will not be stored by the web application.

Relationships
#############

The access point belongs to an organisation and at the same time can have several items (devices and services).

======== =====================================
Relation Description
======== =====================================
Items    All the smart components and services
======== =====================================

In the example picture it is possible to see the count of items for each infrastructure, in addition, each item has in the profile the name and AGID of its agent/adapter. For system integrators using the Open Gateway API there are calls that return all items registered under the agent/adapter among other interesting options.

Manage
######

Here we will focus on the highlighted area of the example picture. Those are the actions that can be done over the infrastructure.

1. In the first place there is creating a new access point. The process is simple just follow these steps:
    * Add a name
    * Select a type
    * Give a password
    * Submit the form

  .. image:: images/nm/img-gat-create.png
     :align: center

2. In the second place, there is a view/modify option. When clicking a new view opens with the access point detail:

  .. image:: images/nm/img-gat-profile.png
     :align: center

If we click over modify the view changes and it is possible to update the name and password of the access point.

  .. image:: images/nm/img-gat-modify.png
     :align: center

3. At last, it is possible to remove a access point from the platform. This means that the platform will forget your infrastructure.
