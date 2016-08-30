Getting Started
===============
DronaHQ provides a robust set of REST APIs and Device APIs that allow mobile developers to quickly add capabilities such as push notifications, or access to device hardware for apps that run inside the DronaHQ Client Container. DronaHQ's APIs are framework independent and can be used by any web application regardless of HTML5 framework(s) that the application is written with.

With DronaHQ you not only can create new applications, but also integrate any existing HTML5 mobile applications to deploy within the DronaHQ platform. 

Whether you are creating a new application on DronaHQ or integrating an existing HTML5 application with DronaHQ , you need to -

	- have a registered |DronaHQ| account.
	- add |dronahq.js|, the DronaHQ javascript SDK, to your application.

.. |DronaHQ| raw:: html

   <a href="http://www.dronahq.com/" target="_blank">DronaHQ</a>
   
.. |dronahq.js| raw:: html

   <a href="https://github.com/dronahq/dronahq.js/releases" target="_blank">dronahq.js</a>

**WHY A REGISTERED ACCOUNT?**

The DronaHQ Administrator Console allows you to create, deploy and manage your application along with many other admin features. So, in order to deploy and manage your applications, you must have a registered DronaHQ account.

A part of the  DronaHQ SDK comes in the form of Web APIs. Authentication of these APIs is done via a **token key**. The **token key** can either be of global/account scope or scoped to the limits of a plugin and can be generated only through the DronaHQ Administrator Console.

|Read more| about the scope of your **token key**.

.. |Read more| raw:: html

   <a href="api-rest-auth.html" target="_blank">Read more</a>
   
**WHY DO I NEED DRONAHQ.JS?**

The dronahq.js provides a set of APIs  that allow an application to interact exclusively with the DronaHQ Container App and also with the device hardware, which makes its inclusion necessary in your application.

.. _quick-start-with-new-app:

Quick start with a new application
----------------------------------

	- |Quick start| by creating a new application that displays the detail of the signed-in user -
	
	.. image:: images/qs.png
		:height: 300px
		:width: 250 px
		:scale: 100 %
		:align: center
		
	- You can also try out more of our |sample applications|.

.. |sample applications| raw:: html

   <a href="https://github.com/dronahq/samples" target="_blank">sample applications</a>
   
.. |Quick start| raw:: html

   <a href="quick-start.html" target="_blank">Quick start</a>

