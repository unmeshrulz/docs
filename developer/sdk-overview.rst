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

	- |User| - allows micro-apps to get the profile of the user currently logged into a Client App.
	- |Notification| - allows a micro-apps to access push notifications received by the Client App.
	- |App| - allows micro-apps to interact with their own instance.
	- |Sync| - allows micro-apps to submit data in background by leveraging offline data upload capability of the Client App.
	- |KVStore| - allows micro-apps to leverage the secure local key-value storage of the Client App as an alternative to the HTML5 local-storage.
	- |SQLite| - provides native interface to SQLite Cordova plugin for Android, iOS, and Windows Phone, with APIs similar to HTML5 or |Web SQL|.

.. |Web SQL| raw:: html

   <a href="http://www.w3.org/TR/webdatabase/" target="_blank">Web SQL</a>
   
Following Device APIs allow micro-apps to access device hardware via the Cordova plugins integrated into our Client App:

	- |Camera| - defines a global navigator.camera object, which provides an API for taking pictures and for choosing images from the device's image library.
	- |File| - implements a File API allowing read/write access to files residing on the device.
	- |FileTransfer| - allows download and upload of files to and from the device.
	- |In App Browser| - provides a web browser view using  :code:`cordova.InAppBrowser.open()` method.

.. |User| raw:: html

   <a href="api-device-user.html" target="_blank">User</a>

.. |Notification| raw:: html

   <a href="api-device-notification.html" target="_blank">Notification</a>
   
.. |App| raw:: html

   <a href="api-device-app.html" target="_blank">App</a>
   
.. |Sync| raw:: html

   <a href="api-device-sync.html" target="_blank">Sync</a>
   
.. |KVStore| raw:: html

   <a href="api-device-kvs.html" target="_blank">KVStore</a>
   
.. |SQLite| raw:: html

   <a href="api-device-sqlite.html" target="_blank">SQLite</a>
   
.. |Camera| raw:: html

   <a href="https://github.com/apache/cordova-plugin-camera" target="_blank">Camera</a>
   
.. |File| raw:: html

   <a href="https://github.com/apache/cordova-plugin-file" target="_blank">File</a>
   
.. |FileTransfer| raw:: html

   <a href="https://github.com/apache/cordova-plugin-file-transfer" target="_blank">FileTransfer</a>
   
.. |In App Browser| raw:: html

   <a href="https://github.com/apache/cordova-plugin-inappbrowser" target="_blank">In App Browser</a>

If you need to use a Cordova plugin outside the above list for your micro-apps, please contact our sales team, and we will be happy to assist.

.. _rest-apis:

REST APIs
---------

The REST APIs  portion of the DronaHQ help micro-apps to interact with the DronaHQ platform for features such as getting user list, sending notifications, etc. DronaHQ makes sure that all **HTTP** request to the REST APIs are authenticated via a **token key** which is either scoped to a channel/account or to a particular micro-app. To generate and learn more about the scope of a **token key**, read our |authentication| guide. 

.. |authentication| raw:: html

	<a href="api-rest-auth.html" target="_blank">authentication</a>
	
The REST APIs include the following methods:

	- |Get Users| - can be used by micro-apps to get a list of users for the given **token key**, ordered chronologically.
	- |Get User| -  can be used by micro-apps to get the user object for a provided **user id** or **email**.
	- |Create Users| - can be used by micro-apps to create users with pre-registered **password** and pre-assigned **groups** based on the scope of a **token key**.
	- |Assign group(s) to a user| - can be used by micro-apps to assign a list of group to a user, and also removes a list of group for a user based on the scope of the **token key**.
	- |Put user(s) to a group| - can be used by micro-apps to assign a list of users to a group, and also remove a list of users from a group based on the scope of a **token key**.
	- |Activate Users| - can be used by micro-apps to activate users based on the scope of the **token key**.
	- |Deactivate Users| - can be used by micro-apps to delete users based on the scope of the **token key**.
	- |Send Notification| - can be used by micro-apps to send notifications to one or more users.
	- |Delete Notification| - can be used by micro-apps to delete notifications on the basis of **notification id** and **content id**.

.. |Get Users| raw:: html

   <a href="api-rest-users.html#get-all-users" target="_blank">Get Users</a>
   
.. |Get User| raw:: html

   <a href="api-rest-users.html#get-user" target="_blank">Get User</a>
   
.. |Create Users| raw:: html

   <a href="api-rest-users.html#create-users" target="_blank">Create Users</a>
   
.. |Assign group(s) to a user| raw:: html

   <a href="api-rest-users.html#assign-grp-user" target="_blank">Assign group(s) to a user</a>
   
.. |Put user(s) to a group| raw:: html

   <a href="api-rest-groups.html#put-users-to-grp" target="_blank">Put user(s) to a group</a>

.. |Activate Users| raw:: html

   <a href="api-rest-users.html#activate-users" target="_blank">Activate Users</a>
   
.. |Deactivate Users| raw:: html

   <a href="api-rest-users.html#deactivate-users" target="_blank">Deactivate Users</a>

.. |Send Notification| raw:: html

   <a href="api-rest-notification.html#send-notification" target="_blank">Send Notification</a>
   
.. |Delete Notification| raw:: html

   <a href="api-rest-notification.html#delete-notification" target="_blank">Delete Notification</a>