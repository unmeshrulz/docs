Overview
========

The DronaHQ Plugin REST APIs provide read/write access to the DronaHQ platform, enabling you to integrate DronaHQ platform with your existing applications or building entirely new micro-apps.

All APIs are |REST|-based. Responses to the Plugin REST APIs  are available in |JSON| format, and all errors are reported via standard HTTP error codes in addition to JSON formatted error information available in the HTTP response body of the relevant requests. 

The DronaHQ REST APIs use an API key for authentication and authorization purposes. DronaHQ provides restricted access to various resources based on the scope of the API key. An API key will have the scope of either:

	- Account
	- Micro-app

To learn more about DronaHQ REST API authentication, please read the |Authentication| section.

.. |REST| raw:: html

   <a href="http://en.wikipedia.org/wiki/Representational_State_Transfer" target="_blank">REST</a>

.. |JSON| raw:: html

   <a href="http://www.json.org/" target="_blank">JSON</a>
   
.. |authentication| raw:: html

   <a href="api-rest-auth.html" target="_blank">authentication</a>
