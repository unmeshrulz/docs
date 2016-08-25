Getting Started
===============
	- :ref:`A new application <quick-start-with-new-app>`
	- :ref:`An existing application <existing-app>`
   
DronaHQ provides robust APIs that allow applications to have various capabilities when it runs inside the DronaHQ Container App. DronaHQ's APIs are framework independent and can be used by any web application regardless of framework(s) that application is written with.

With DronaHQ you can not only create new applications, but seamlessly integrate your exisitng HTML5 mobile application with the DronaHQ platform. 

Whether you are creating a new application on DronaHQ or integrating an existing HTML5 application with DronaHQ , you need to have the following prerequisites -

	- A registered |DronaHQ| account.
	- |dronahq.js| - the DronaHQ javascript SDK.

.. |DronaHQ| raw:: html

   <a href="http://www.dronahq.com/" target="_blank">DronaHQ</a>
   
.. |dronahq.js| raw:: html

   <a href="https://github.com/dronahq/dronahq.js" target="_blank">dronahq.js</a>

**Why do you need A registered DronaHQ account?**

The DronaHQ Administrator Console allows you to create, deploy and manage your application along with many other admin features. So, it goes without saying that you must have a registered DronaHQ account.

A part of the  DronaHQ SDK comes in the form of Web APIs. Authentication of these APIs is done via a *tokenkey*. The *tokenkey* can either be of global/account scope or scoped to the limits of a plugin and can be generated only through the DronaHQ Administrator Console.

|Read more| about the scope of your *tokenkey*.

.. |Read more| raw:: html

   <a href="api-rest-auth.html" target="_blank">Read more</a>
   
**Why do you need the dronahq.js?**

The dronahq.js provides a set of APIs  that allow an application to interact exclusively with the DronaHQ Container App and also with the device hardware, which makes its inclusion necessary in your application.

.. _quick-start-with-new-app:

A new application
----------------------------------

	- |Quick start| by creating a new application that displays the message -
	
	.. image:: images/sample-qs.png
		:height: 100px
		:width: 500 px
		:scale: 100 %
		:align: center
		
	- You can also try out more of our |sample applications|.

.. |sample applications| raw:: html

   <a href="https://github.com/dronahq/samples" target="_blank">sample applications</a>
   
.. |Quick start| raw:: html

   <a href="quick-start.html" target="_blank">Quick start</a>
	
.. _existing-app:

An existing application
----------------------------------

