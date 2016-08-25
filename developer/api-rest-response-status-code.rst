.. _ref-rest-api-err:

Error Handling
======================
Response Status Codes
-------------------------------------------------------------
The DronaHQ APIs uses the following HTTP status codes in its response:

+-------------+-------------------------------------------------------------------------------------------+
| Status Code | Description                                                                               |
+=============+===========================================================================================+
| 200         | OK - The request has succeeded. The client can read the result of the request in the body.|
+-------------+-------------------------------------------------------------------------------------------+
| 204         | No Content - The request has succeeded but returns no message body.                       |             
+-------------+-------------------------------------------------------------------------------------------+
| 400         | Bad Request - The request could not be understood by the server due to malformed syntax.  |
|             | The message body will contain more information.                                           |
+-------------+-------------------------------------------------------------------------------------------+
| 401         | Unauthorized - The authorization is refused for the resource with the provided API key.   |
+-------------+-------------------------------------------------------------------------------------------+
| 403         | Forbidden - The server understood the request, but is refusing to fulfill it.             |
+-------------+-------------------------------------------------------------------------------------------+
| 404         | Not Found - The requested resource could not be found. This error can be due to a         |
|             | temporary or permanent condition.                                                         |
+-------------+-------------------------------------------------------------------------------------------+
| 500         | Internal Server Error. You should never receive this error because our clever coders catch|
|             | them all. But if you are unlucky enough to get one, please report it to us through our    |
|             | support desk at |friends@dronahq.com|.                                                    |
+-------------+-------------------------------------------------------------------------------------------+
| 503         | Service Unavailable - The server is currently unable to handle the request due to a       |
|             | temporary condition which will be alleviated after some delay. You can choose to resend   |
|             | the request again.                                                                        |
+-------------+-------------------------------------------------------------------------------------------+

.. |friends@dronahq.com| raw:: html

	<a href="mailto:friends@dronahq.com">friends@dronahq.com</a>
	
In case of error response, you can also check the response body to get more information about the error.

Error Response Body
-------------------
+-----------+--------------------------------------+
| Parameter | Description                          |
+===========+======================================+
| error     | Error Code                           |
+-----------+--------------------------------------+
| message   | Textual description of the error.    |
+-----------+--------------------------------------+
| reason    | Probable cause/reason for the error. |
+-----------+--------------------------------------+