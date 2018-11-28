This section describes the item entity. In a real infrastructure, an item can be a smart device or a value-added service.

All the items need an adapter/agent and a gateway to be integrated in the platform. Once registered, users with service provider or device owner roles will be responsible for the services and devices respectively.

Properties
##########

In the table is possible to see the most relevant properties, for those which are possible to update there will be a separate sub-section in the last section of this page ("Manage").

========== ====================================== ======
Property   Description                            Update
========== ====================================== ======
Name       Friendly name                          NO
OID        External ID                            NO
Type       Indicates if it is a service or device NO
Visibility Private, for friends, public           YES
Info\*     Thing description of the item          YES
Avatar\*\* Item logo                              YES
Status     Enabled/Disabled                       YES
========== ====================================== ======

\* It can only be updated with the adapter/agent
\*\* Can be updated clicking over it and selecting a new picture from the folder

.. note:: Info property: Thing description of the item In the item profile, there is a tab call "Description" that shows a series of collapsible tiles containing all the properties that define the Thing Description of the item. We can find there all the available properties and actions for the given item. This properties, actions and events are the same that can be requested using the consumption endpoints of the `Open Gateway API <https://github.com/vicinityh2020/vicinity-neighbourhood-manager/wiki/TBD>`_

.. image:: images/nm/img-items-description.png
   :align: center
   :width: 600px
   :height: 600px

Relationships
#############

The items belong to a gateway and to a user. It can have contracts and audits.

========= ==========================================
Relation  Description
========= ==========================================
Contracts Agreement to share device data
Audits    Most relevant events of the item (History)
========= ==========================================

Manage
######

See the platform items In the sidebar menu, there are two tabs for the items: Devices (Orange widget) and services (Yellow widget). These views show your items, public items and items of your friends that are flagged as "visible for friends". There are as well filtering options.

.. image:: images/nm/img-items-sidebar-view.png
   :align: center
   :width: 1000px
   :height: 400px

Each widget contains certain information about the item. We can see in the device widget a yellow-highlighted area with the item name and the organisation that owns the item. Right below there is a green-highligted are showing the status and visibility of the item. There is also an avatar on the top left corner and the logo type of adapter that has integrated the item on the right bottom corner. Finally, at the bottom there is a line with the owner of the item and a link to the profile.

.. figure:: images/nm/img-items-device-widget.png
   :align: center

   Device widget

In the service widget there is also a green action button in the case that the service is possible to request. Pressing *\+Request Service* would initiate the contract process.

.. figure:: images/nm/img-items-service-widget.png
   :align: center

   Service widget

Status & visibility
###################

An item can go through several status and visibility levels, see the table below to understand some of the possible actions and effects in the item lifecycle.

=================================== =================== ======== ========== ========
Action                              Visibility          Status   hasGateway hasOwner
=================================== =================== ======== ========== ========
Agent registers item in platform    Private             Disabled YES        NO
User enables item                   Private             Enabled  YES        YES
User makes item visible for friends Visible for friends Enabled  YES        YES
User makes item public              Public              Enabled  YES        YES
User disables item                  Private             Disabled YES        NO
User deletes item                   N/A                 Deleted  NO         NO
=================================== =================== ======== ========== ========

.. note:: IMPORTANT: In order to assign a visibility level the device must be enabled. As well, it is important to remember that the visibility level of a device must remain lower than the visibility of its user owner.

Now we will see the all possible status and visibility levels and how to change them from the item profile.

1. Status There are three possible status for an item:
    * Disabled: When an item has just been registered, it appears as disabled in the platform. An item can be disabled by its owner anytime. Being disabled means that the item is private and is not visible in the P2P network. Furthermore a disabled item does not have owner assigned in the platform.
    * Enabled: When a user claims an item, this turns to the enabled status. Now it is possible to change the visibility level and use it in contracts. It is possible to see the following changes in the "Disabled vs Enabled" picture.
        * New section with visibility settings
        * User owner of the item added
        * Button to change user enabled
        * New section with a button to remove the device

.. figure:: images/nm/img-items-compare.png
   :align: center
   :width: 700px
   :height: 700px

   Disabled vs Enabled

    * Deleted: The owner of the item can decide to remove it from the platform. This will remove the item from all existing contracts and it will make it no longer visible in the platform. This action can also be triggered using the agent.

.. image:: images/nm/img-items-remove.png
   :align: center
   :width: 300px
   :height: 600px

2. Visibility

There are also three visibility levels:
* Private: Only my organisation can see the item.
* Visible for friends: Other organisations that are partners with mine can see the item and request its data making use of a contract.
* Public: All organisations can see the item and request its data making use of a contract.

To change the visibility go to the item profile and use the option highlighted. Remember to enable the device and set the visibility of your user adequately. See the process below:

.. image:: images/nm/img-items-visibility.png
   :align: center
   :width: 600px
   :height: 600px

Move an item
############

It is possible to change the owner or gateway of an item without interfering existing contracts or doing any additional changes. For that it is necessary to be the current owner of the device or an organisation administrator. Nevertheless, there are some conditions that have to be met:

* Case of changing a gateway: New gateway needs to have the same type. I.e: Vicinity agent to Vicinity agent.

* Case of changing owner: New user need to have device owner or service provider role (Depending on the item you are moving). The privacy level of the item you are moving will be reduced if the new user has lower visibility, be aware that this can cause that your device will be removed from some contracts. *Check the visibility of the new user
to avoid issues.*

Moving an item can be specially useful in the situation of a leaving employee or change of responsibilities within an organisation. The status of the items under the responsibility of this person will remain unchanged, but we can have a different person in charge.

To move an item go to the item profile and select one of the options in the move item menu. When you click over change owner or change gateway, you will see a dropdown menu where you can select the desired option and finish the process by clicking save.

.. note:: Note that if the device is disabled you will only be able to change the gateway because there is no item owner at the moment.

.. image:: images/nm/img-items-move.png
   :align: center
   :width: 600px
   :height: 600px
