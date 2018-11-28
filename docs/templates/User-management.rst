An administrator has certain control over its organisation users. As of
now, an administrator can remove or change the roles of a user.

.. note:: Important notes regarding change of roles and user removal: The rules below apply both the case that we want to remove a user or just change a role.

* There must be in all moments at least one organisation administrator.

* There are some roles that cannot be removed if the user is still bound to some of its responsibilities, i.e:

    * A device owner cannot be removed until the user does not have any devices under its responsibility. The devices can be either deleted, disabled or moved to another user.
    * A service provider cannot be removed until the user does not have any services under its responsibility. The services can be either deleted, disabled or moved to another user.
    * A infrastructure operator cannot be removed until all contracts it is responsible for are either moved to other infrastructure operator or cancelled.

Remove user
###########

It is possible to remove users from the organisation profile, under the tab role management.

.. image:: images/nm/img-org-roles-remove.png
   :align: center

Change roles
############

As mentioned, only administrators can add/remove roles. Otherwise the process is simple and it is located under the organisation profile.

.. image:: images/nm/img-org-role-mgmt.png
   :align: center

.. image:: images/nm/img-org-role-mgmt-update.png
   :align: center

.. note:: In order for the changes to take effect, the user that had its roles changed might need to log in again.
