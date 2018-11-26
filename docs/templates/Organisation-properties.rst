The organisation is the main entity in the platform. Under the organisation there can be registered several users and gateways. Everything in the in the platform has to belong to an organisation.

To characterise an organisation there is a series of properties and relationships.

Properties
----------

============ ==================================== ======
Property     Description                          Update
------------ ------------------------------------ ------
Name         Organisation name                    NO
CID          External ID                          NO
BID\*        Business ID                          NO
Avatar       Organisation logo                    YES
Location     Country or city                      YES
Notes        Information about the organisation   YES
Skin color   Account app colour                   YES
============ ==================================== ======

.. note:: If your organisation/association lacks BID or your are a legal person you might choose a random number or string.

Some of the properties can be modified/updated. To do so navigate to the organisation profile and update them using the edit buttons. The avatar can be updated by clicking over it. Note that **_in order to update the organisation profile you need to have the administrator role._**

.. image:: images/nm/img-org-profile.png
   :align: center

Relationships
-------------

The organisation is linked to other entities as previously mentioned.

============= ==========================================================================
Relation      Description
------------- --------------------------------------------------------------------------
Gateways      All the infrastructures registered under the organisation
Users         All the users registered under the organisation
Partners      Other organisations that agreed to be partners
Notifications Some actions trigger notifications that are sent to the whole organisation
Audits        Keeps track of relevant events for the organisation (History)
============= ==========================================================================

To visually see all these relationships in the UI is very easy, see below how:

* Partners, audits and users: Directly in the organisation profile.

.. image:: images/nm/img-org-profile-relations.png
   :align: center

* Gateways: In the side menu there is a tab dedicated to them. You need to be system integrator to see this menu.

.. image:: images/nm/img-gateways.png
   :align: center

* Notifications: The bell on the top menu shows all notifications addressed to the organisation.

.. image:: images/nm/img-notifications.png
   :align: center
