Notification
===============
This family of DronaHQ Plugin REST APIs provide access to the DronaHQ notifications.
It provide methods to perform the following action(s)-

	- :ref:`send-notification`
	- :ref:`delete-notification`
	
.. _send-notification:

Send Notification
------------------

This API can be used to send notification to one or more users within the |scope| of its *tokenkey*.

.. |scope| raw:: html

   <a href="api-documentation-plugin-rest-auth.html" target="_blank">scope</a>
   
**ENDPOINT**
:code:`PUT https://plugin.api.dronahq.com/v2/notifications`

**Request Parameters**

**Request Body Data**

+-----------+-------------+-------------------------------------------------------------------------------------------------------------+
| Parameter | Value Type  | Description                                                                                                 |
+===========+=============+=============================================================================================================+
| token_key | string      | Your API key. Check |authentication| for more details.                                                      |
+-----------+-------------+-------------------------------------------------------------------------------------------------------------+
| user_id   | array       | *Required.* A string array of the users DronaHQ ID. A maximum of 50 IDs can be sent in one request.         |
+-----------+-------------+-------------------------------------------------------------------------------------------------------------+
| message   | string      | *Required.* Text message to be sent as notification. A maximum of 160 characters are accepted as message.   |
+-----------+-------------+-------------------------------------------------------------------------------------------------------------+
| data      | string      | *Optional.* A custom JSON string to be sent along with the notification. This data can be retrieved using   |
|           |             | device API of `dronahq.js`.                                                                                 |
+-----------+-------------+-------------------------------------------------------------------------------------------------------------+

.. |authentication| raw:: html

   <a href="api-rest-auth.html" target="_blank">authentication</a>

**RESPONSE FORMAT**

On error, the header status code is an error code and the response body contains an |error object|.

.. |error object| raw:: html

   <a href="api-documentation-plugin-rest-response-status-code.html#error-response-body" target="_blank">error object</a>

On success, the HTTP status code in the response header is 200 OK and the response body contains an JSON object.

**Success Response Data**

+--------------+------------+-----------------------------------------------------------------------------------------------------------------+
| Parameter    | Value Type | Description                                                                                                     |
+==============+============+=================================================================================================================+
| noti_id      | integer    | Unique ID of the sent notification. <br> This ID can be used to later delete the notification from users inbox. |
+--------------+------------+-----------------------------------------------------------------------------------------------------------------+
| user_success | array      | A string array of user IDs to whom the notification has been sent successfully.                                 |
+--------------+------------+-----------------------------------------------------------------------------------------------------------------+
| user_failed  | array      | A string array of invalid user IDs to whom notifications could not be delivered.                                |
+--------------+------------+-----------------------------------------------------------------------------------------------------------------+

.. _delete-notification:

Delete Notification
--------------------

This API can be used to deletes a notification on the basis of notification id and content id within the |scope| of its *tokenkey*.

**ENDPOINT**
:code:`DELETE https://plugin.api.dronahq.com/notifications/{noti_id}`

**REQUEST PARAMETERS**

**URL Segment**

+-----------+-------------+-------------------------------------------------+
| Parameter | Value Type  | Description                                     |
+===========+=============+=================================================+
| noti_id   | integer     | Unique ID of the notification to be deleted.    |
+-----------+-------------+-------------------------------------------------+

**Query string**

+-----------+-------------+-------------------------------------------------------+
| Parameter | Value Type  | Description                                           |
+===========+=============+=======================================================+
| token_key | string      | Your API key. Check |authentication| for more details.|
+-----------+-------------+-------------------------------------------------------+

Response Format
--------------------

On error, the header status code is an error code and the response body contains an |error object|.

On success, the HTTP status code in the response header is 204 No Content.