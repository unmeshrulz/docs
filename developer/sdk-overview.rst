.. _sdk-overview:

DronaHQ SDK Overview
====================
The DronaHQ SDK comprises of two major segments. Each segment contains  powerful resources to help you build engaging applications on the DronaHQ platform.

	- :ref:`device-apis`
	- :ref:`rest-apis`

.. _device-apis:	

Device APIs
-----------

The Device APIs portion of the SDK provides a set of APIs that allow micro-apps to interact with the DronaHQ Client Container App as well as the device hardware features such as Camera, File System, etc.

Following Device APIs allow micro-apps to interact with the DronaHQ Client Container App:

	- **User** - allows micro-apps to get the profile of the user currently logged into a Client App.
	- **Notification** - allows a micro-apps to access push notifications received by the Client App.
	- **App** - allows micro-apps to interact with their own instance.
	- **Sync** - allows micro-apps to submit data in background by leveraging offline data upload capability of the Client App.
	- **KVStore** - allows micro-apps to leverage the secure local key-value storage of the Client App as an alternative to the HTML5 local-storage.
	- **SQLite** - provides native interface to SQLite Cordova plugin for Android, iOS, and Windows Phone, with APIs similar to HTML5 or Web SQL (http://www.w3.org/TR/webdatabase/).

Following Device APIs allow micro-apps to access device hardware via the Cordova plugins integrated into our Client App:

	- `Camera`_ - defines a global navigator.camera object, which provides an API for taking pictures and for choosing images from the device's image library.
	- `File`_ - implements a File API allowing read/write access to files residing on the device.
	- `FileTransfer`_ - allows download and upload of files to and from the device.
	- `In App Browser`_ - provides a web browser view using  cordova.InAppBrowser.open() method.

.. _Camera: https://github.com/apache/cordova-plugin-camera
.. _File: https://github.com/apache/cordova-plugin-file
.. _FileTransfer: https://github.com/apache/cordova-plugin-file-transfer
.. _In App Browser: https://github.com/apache/cordova-plugin-inappbrowser

If you need to use a Cordova plugin outside the above list for your micro-apps, please contact our sales team, and we will be happy to assist.

.. _rest-apis:

REST APIs
---------

The REST APIs  portion of the DronaHQ help micro-apps to interact with the DronaHQ platform for features such as getting user list, sending notifications, etc. DronaHQ makes sure that all *HTTP* request to the REST APIs are authenticated via a *token key *which is either scoped to a channel/account or to a particular micro-app. To generate and learn more about the scope of a *token key*, read our authentication guide (https://quip.com/uuPaAapx9hc2). 

The REST APIs include the following methods:

	- **Get Users** - can be used by micro-apps to get a list of users for the given *token key*, ordered chronologically.
	- **Get User** -  can be used by micro-apps to get the user object for a provided *user id* or *email*.
	- **Create Users** - can be used by micro-apps to create users with pre-registered *password* and pre-assigned *groups* based on the scope of a *token key*.
	- **Put user(s) to a group** - can be used by micro-apps to assign a list of users to a group, and also remove a list of users from a group based on the scope of a *token key*.
	- **Assign group(s) to a user** - can be used by micro-apps to assign a list of group to a user, and also removes a list of group for a user based on the scope of the *token key*.
	- **Deactivate Users** - can be used by micro-apps to delete users based on the scope of the *token key.*
	- **Activate Users** - can be used by micro-apps to activate users based on the scope of the *token key.*
	- **Send Notification** - can be used by micro-apps to send notifications to one or more users.
	- **Delete Notification** - can be used by micro-apps to delete notifications on the basis of *notification id* and *content id.*



