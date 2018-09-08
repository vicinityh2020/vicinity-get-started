===================
VICINITY Vocabulary
===================

.. glossary::

  VICINITY Cloud
    The VICINITY Cloud enables IoT infrastructure operators and Service providers to configure a virtual neighbourhood of connected devices and value-added services including the setup of sharing access rules between them through the user-friendly interface of VICINITY Neighbourhood Manager accessible through http://vicinity.bavenir.eu.

  VICINITY Node
      A VICINITY Node is the set of software components which maintains the user data exchange between peers in the VICINITY P2P network based on configuration of the virtual neighbourhood and sharing rules received from VICINITY Communication Server. VICINITY Node is collection of software components (VICINITY Gateway API, VICINITY Agent and VICINITY Adapter) enables your IoT infrastructure or services communicates with other VICINITY enabled devices and services.

  VICINITY Adapter
    The *VICINITY Adapter* is a component provided by IoT infrastructure owner/ vendor or respective system integrator. The VICINITY Adapter provides translates the VICINITY "world" into the IoT infrastructure specific environment. The core responsibilities of the Adapter are:

    * to provide the description of each IoT objects (devices, services) of infrastructure in common VICINITY format, which enables VICINITY to create internal models of used IoT objects in uniform way;
    * to provide access to properties, actions and events of devices and services provided by infrastructure.

  VICINITY Gateway API
    The *VICINITY Gateway API* provides services to access VICINITY Peer-to-peer network. It is primary interface towards VICINITY Platform.

  VICINITY Agent
    The *VICINITY Agent* is the wrapper for the VICINITY Adapter. The VICINITY Agent provides the full VICINITY specific functionality, as managing communication via P2P network, semantic discovery of IoT objects (devices and services), semantic search of IoT objects, communication with IoT objects within the infrastructure, where each VICINITY specific interaction with IoT objects is translated in the VICINITY Adapter calls.
    In general, *VICINITY Agent* simplifies the communication with VICINITY Gateway API especially with IoT objects (device and services) registration and update.

  Agent ID (agid)
    The Agent ID is UUID (VICINITY Platform unique key) which represents adapter in VICINITY peer-to-peer network. The agent Id is generated during registration of the access point in VICINITY Neighbourhood manager (https://github.com/vicinityh2020/vicinity-neighbourhood-manager/wiki/Access-points). The agent ID is used during login of the VICINITY Adapter into the peer-to-peer network and during registration of IoT objects.

  Object ID (oid)
    The Object ID is UUID (VICINITY Platform unique key) which represents any device and service registered in VICINITY. This ID is used to access devices and services properties, actions and events. The ID is generated during device and service registration via VICINITY Gateway API.

  Property ID (pid)
    The Property ID is UUID (IoT object unique key) which represents any property provided by the device or service.

  Action ID (aid)
    The Action ID is UUID (IoT object unique key) which represents any action provided by the device or service.

  Event ID (eid)
    The Event ID is UUID (IoT object unique key) which represents any event provided by the device or service.

  IoT Object
    The IoT object is any device or service registered in VICINITY Platform. The IoT object is referenced by uniq *oid* generated during its registration. The IoT object is represented by Thing description. In terms of devices, the IoT object represents a virtual device.

  Virtual device
    The virtual device represents set of device(s) capabilities (such as current temperature, on/off switch, etc.). The set of device(s) capabilities can be represented in the real world as follows:
    * 1-on-1: virtual device have the same set of capabilities as physical one represented by the virtual one;
    * subsetting: virtual device have the subset of physical device capabilities, Note that capabilities which are not provided by virtual device will not be accessed through the VICINITY;
    * union: virtual device represents the multiple physical devices in one virtual device.

    .. attention::
      Currently VICINITY support any access restrictions on the level of virtual device. Enabling access to union virtual device results in access to all related physical devices by design. Enabling access to 1-on-1 virtual device results in access to all capabilities of physical device. Hence it is advised to follow subsetting virtual device approach.

    The virtual device is represented by one Thing description.

  Thing description (TD)
    The thing description is JSON object providing identification of IoT object and description of each IoT object capabilities (properties, actions and events) (https://github.com/vicinityh2020/vicinity-agent/blob/master/docs/TD.md ).
