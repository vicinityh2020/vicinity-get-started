This section covers all user properties and relations to other entities.
It will deal separately with the **roles**, since it is the most
complex part of a user. An organisation can have many users, and each of
them can play one or several roles. Managing infrastructures, services
or the organisation account are some of the tasks.

Properties
##########

============ ================================================= ======
Property     Description                                       Update
------------ ------------------------------------------------- ------
Name         User name                                         NO
Email        Registration mail and external ID                 NO
Contact Mail E-mail address to contact user after registration YES
Avatar       User logo                                         YES
Password     Credentials to log in the platform                YES
Occupation   Information about the organisation                YES
Visibility\* Who can see the user in the platform              YES
============ ================================================= ======

.. note:: The visibility of the user has to be always higher than the visibility of its devices and services. Therefore if there is an attempt to give a device public visibility while the user is private or only visible to friends, there will be a warning notification stating the issue.

Some of the properties can be modified/updated. To do so navigate to the
user profile and update them using the edit buttons. The avatar can be
updated by clicking over it.

.. note:: Note that only a user can update its own properties. An exception to this rule are the roles, which will be explained in the next section.

.. image:: https://github.com/vicinityh2020/vicinity-neighbourhood-manager/wiki/Images/img-user-properties.png
   :align: center

Relationships
#############

The user is linked to other entities as previously mentioned.

============= ===================================================
Relation      Description
============= ===================================================
Items         All the devices and services managed by the user
Contracts     Agreements to share data where the user participates
Notifications Some actions trigger notifications that are sent to a single user
Audits        Keeps track of relevant events for the user (History)
============= ===================================================

To visually see all these relationships in the UI is very easy, see
below how:

* Items, audits and contracts: Directly in the user profile.

.. image:: https://github.com/vicinityh2020/vicinity-neighbourhood-manager/wiki/Images/img-user-profile-relations.png
   :align: center

* Notifications: The bell on the top menu shows all notifications addressed to the user.

.. image:: https://github.com/vicinityh2020/vicinity-neighbourhood-manager/wiki/Images/img-notifications.png
   :align: center

Roles
#####

There are 6 user roles in the platform:

* User: All users have this role by default
* Administrator: The user that registers an organisation gets this role by default. It can be also granted to other users afterwards. It is necessary to be administrator in order to:
    * Modify organisation properties
    * Remove and organisation
    * Update users roles
* Device owner: A user with this role can enable devices. When enabling a device, the user becomes its responsible platform-wise and it can manage its properties and share its data.
* Service provider: A user with this role can enable services. When enabling a service, the user becomes its responsible platform-wise and it can manage its properties and share its data.
* System integrator: A user that can manage the gateways (agents/adapters) of the platform. Can create, update and remove gateways.
* Infrastructure operator: A user that can initiate contracts. In other words the responsible to choose which service can adequate to some of the devices of its organisation and request it.
The infrastructure operator can be the owner of devices or not (having the role of device owner). In case the devices chosen to be part of a contract do not belong to the infrastructure operator, the actual owners will be asked for permission. The contract will be completed only with those devices whose owners agreed. Additionally there is a role granted by the platform administrator: \
* superUser: This role is reserved for developers interested in using the API. With this role, it is possible to use a faster registration method.
