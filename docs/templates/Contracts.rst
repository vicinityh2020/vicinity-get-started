This section covers one of the last additions to the platform, the contracts. Through contracts is how devices and services of different organisations can share data and interact.

Currently there is only one contract type "Service Request". In this type of contract, services and devices from organisation_one can request to use a service from organisation_two. This service will be monitoring (or monitoring and controlling) those items that were added to the contract by organisation_one.

In order to create a contract we need to follow these steps:

* A user with role *Infrastructure Operator* selects a service (public or from a partner organisation)
* Then the *Infrastructure Operator* needs to choose the contract settings:
    * Select items (with fitting visibility) from his organisation. Can be from different users.
    * Choose if we want to give write rights to the service or not.
    * Approve the terms and conditions.
* A notification will be sent to the service owner.
* Once the service provider approves the contract, the service can start sharing data with the items belonging to the *Infrastructure Operator*, if he had any. All other items that belong to different users will not be part of the contract until their owners give consent.
* Therefore, the next step is to send a notification to all users so they can approve or reject the contract.
* In the end the contract is completed with the *Infrastructure Operator*, *Service Provider* and all other users that gave consent to share data.
* Any time a user can withdraw from the contract. In the subsequent points we will define the properties, relations and actions that the contract has got in order to fulfil its purpose.

.. image:: images/nm/img-contracts-main.png
   :align: center
   :width: 300px
   :height: 600px

.. note::

    Important notes:
        * To request a service you need to be IoT operator.
        * If the contract includes devices that belong to your organisation, but you are not the owner, then the respective owners will need to agree the contract. Meanwhile, these devices will appear as inactive or disabled, and will not be reachable by the service.
        * All users with devices/services in the contract, will see the contract in their respective contracts view.
        * The IoT operator and the service provider are considered administrators of the contract. They can cancel the contract anytime unilaterally. If a user that is not admin, presses the button to remove the contract, only itself and its devices will be removed. The contract will remain with the rest of the users.
        * The items involved in the contract can be managed individually, but only by their owners.

Properties
##########

In the table is possible to see the most relevant properties, for those which are possible to update there will be a separate sub-section in the last section of this page ("Manage").

============= ===================================================== ======
Property      Description                                           Update
============= ===================================================== ======
CTID          External ID                                           NO
IoT Owner\*   Object with relevant info of the infrastructure owner YES
Foreign IoT\* Object with relevant info of the service owner        YES
Status        Active or deleted                                     YES
R/W           Monitor or monitor & control                          NO
T&C           Terms and conditions                                  YES
Type          Right now, only service request                       NO
============= ===================================================== ======

*(\*)* IoT Owner and foreign IoT contain:
    * Users with items involved in the contract;
    * Information about the organisations the users belong to;
    * Items in the contract with a flag stating whether they are active or not;
    * Flag indicating if the terms and conditions were accepted. Only the IoT Provider and the Service Provider need to approve the T&C on behalf of the rest of users. However, each user can decide if its devices are included in the contract.

Relationships
#############

Each item and user involved in a contract has information of it stored in an array of contracts.

================ ======================================================================= ======
Property         Description                                                             Update
================ ======================================================================= ======
CTID             External ID                                                             NO
ID               Mongo ID                                                                NO
imForeign        If true, user is service provider                                       NO
imAdmin          If true, user is IoT Operator (or Service Provider if ImForeign = TRUE) NO
approved         Indicates whether the user has agreed T&C                               YES
inactive         Array with user inactive devices                                        YES
readWrite        Contract with monitor or control capabilities                           NO
contractingUser  ID of the administrator of the other side of the contract               NO
contractingParty ID of the organisation of the other side of the contract                NO
================ ======================================================================= ======

Main views
##########

*Request contract*

To access this view a user with IoT Operator role needs to click on the "request service" button of any eligible service.

1. Read the description of the service you are requesting.
2. Select the devices/services you would like to include in the contract.
3. Grant control or control&monitoring permissions to the service over your infrastructure.
4. Read and accept the terms and conditions.

.. image:: images/nm/img-contracts-request.png
   :align: center
   :width: 600px
   :height: 600px

*All contracts view*

To access this view users with "device owner", "service provider" or "IoT Operator" roles, just need to click on the sidebar the contracts link.
1. See if your organisation is providing (red flag) or using the service (green flag).
2. See some important information: Service name, service provider name, owner of the infrastructure, number of items in the infrastructure, status of the contract, and type and permissions.
3. Actions: Approve, see details and cancel contract.

.. image:: images/nm/img-contracts-view.png
   :align: center
   :width: 1000px
   :height: 200px

*Contract detail*

To access this view users with "device owner", "service provider" or "IoT Operator" roles, just need to click on the eye icon that you will find in each contract instance of the "all contracts view".
1. Information of the service provider and service used.
2. Legal description of the contract.
3. Items info:
    * Name, OID, type of item, owner, status and actions.
    * Actions are affecting status of the items in the contract. It is possible to enable, disable and remove item from the contract.
4. Contract action buttons:
* Transfer contract, accept contract, remove contract and close details.

.. image:: images/nm/img-contracts-detail.png
   :align: center
   :width: 1000px
   :height: 500px

Manage
######

*Create a contract*

1. Select a service and click "Request service". Do it from the services view or from a service profile.
2. The "request service view" opens, there you can see all the information about the service you are requesting and select the items that you would like to share with it. Also it is possible to grant read or read/write permissions to the service over your devices.
3. Once you agree the terms and conditions, it is possible to submit the contract request.
4. Now the contract appears in the "all contracts view" (sidebar menu). The contract will be active once the service provider accepts the request.
5. All parties should receive notifications with the status of the contract.

.. image:: images/nm/img-contracts-request-service.png
   :align: center
   :width: 1000px
   :height: 500px

*Accept, see or remove a contract*

Within the "all contracts view" you have several actions for each contract.

* See details: Represented by an eye. Opens the "contract detail view".
* Accept a contract: Green check sign.
* If you are an administrator: Accepts contract.
* If you are a normal user: Enables your devices in the contract.
* Remove contract: Red cross.
* If you are an administrator: Removes contract.
* If you are a normal user: Removes you and your items from the contract.

.. image:: images/nm/img-contracts-actions.png
   :align: center
   :width: 400px
   :height: 200px

*Transfer a contract*
It is possible to transfer the responsibility of being the administrator of a contract. To do so click in the exchange button in the "contract detail view" and select a suitable recipient for the responsibility. That would be some user with the IoT Operator role.

.. image:: images/nm/img-contracts-actions-move.png
   :align: center
   :width: 400px
   :height: 100px

*Enable, disable and remove devices within a contract*
Each item can be managed individually by its owner. To do so, navigate to the "contract detail view" and at the bottom you will see all the items. Only those which are your will have actions available.

Enabling (green check) and disabling (pause sign) keep the item in the contract. A disabled/inactive device cannot be reached by the service.

Removing (red cross) a device, deletes all links of the device with the contract. After removing the device it is not possible to enable it back.

.. image:: images/nm/img-contracts-device-actions.png
   :align: center
   :width: 400px
   :height: 200px

Update items in the contract
############################

A system integrator may update devices or services in the infrastructure, and these items might be involved in some contracts. This could be an issue, since the updated item might have new properties or actions that were not included in the terms of conditions of previously existing contracts. To cope with this problem, the update process modifies the contracts in the following manner:

* When updating a device:
    1. The device is put to disabled mode in all contracts.
    2. Contracts flag the device as inactive.
    3. The owner of the device is notified and then it is possible to revise the T&C and accept or reject them.
    4. If the device owner accepts the conditions the device will be added to the contract again, if not, will be permanently removed.
* When updating a service:
    1. The contract is moved to the state when all parties need to approve the T&C, it suffers a reset.
    2. The items included in the contract cannot communicate anymore, until their owners approve the new T&C. 3. All the users involved in the contract are notified, and once they approve the T&C, the contract is re-established.
