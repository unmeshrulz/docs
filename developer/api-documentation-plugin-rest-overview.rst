Overview
========

The DronaHQ Plugin REST APIs provide read/write access to the DronaHQ platform, enabling you to integrate DronaHQ platform with your existing applications or building entirely new micro-apps.

All APIs are `REST`_-based. Responses to the Plugin REST APIs  are available in `JSON`_ format, and all errors are reported via standard HTTP error codes in addition to JSON formatted error information available in the HTTP response body of the relevant requests. 

.. _REST: http://en.wikipedia.org/wiki/Representational_State_Transfer
.. _JSON: http://www.json.org/

The DronaHQ REST APIs use an API key for authentication and authorization purposes. DronaHQ provides restricted access to various resources based on the scope of the API key. An API key will have the scope of either:

	- Account
	- Micro-app

To learn more about DronaHQ REST API authentication, please read the :ref:`Authentication <ref-rest-api-auth>` section.
