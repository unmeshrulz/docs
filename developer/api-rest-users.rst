Users
======
DronaHQ Plugin REST API provide read/write access to DronaHQ user(s) through the following action(s)-

	- :ref:`get-all-users`
	- :ref:`get-user`
	- :ref:`create-users`
	- :ref:`assign-grp-user`
	- :ref:`activate-users`
	- :ref:`deactivate-users`
	
.. _get-all-users:

Get all users
--------------

This API returns a list of users within the |scope| of the *tokenkey*, ordered chronologically.

.. |scope| raw:: html

   <a href="api-documentation-plugin-rest-auth.html" target="_blank">scope</a>
   
**ENDPOINT** 

.. code-block:: text 

	GET /users

**REQUEST PARAMETERS**

**Query string**

+-----------+------------+----------------------------------------------------------------------------------------------------+
| Parameter | Value Type | Description                                                                                        |
+===========+============+====================================================================================================+
| tokenkey  | string     | *Required.* Your API key. Check |authentication| for more details                                  |
+-----------+------------+----------------------------------------------------------------------------------------------------+
| ulimit    | integer    | *Optional.* The number of users to return. Default is 25.                                          |
+-----------+------------+----------------------------------------------------------------------------------------------------+
| maxuid    | integer    | *Optional.* If given, users with user id greater than the *maxuid* are returned. To use this       |
|           |            | argument for paging, you can use the user_id field in the returned user objects.                   |
+-----------+------------+----------------------------------------------------------------------------------------------------+
| gid       | integer    | *Optional.* If given, users who belong to the group with id *gid* are returned.                    |
+-----------+------------+----------------------------------------------------------------------------------------------------+
| search    | string     | *Optional.* Keyword to search for in user's name. Only users matching with the query will be       |
|           |            | returned.                                                                                          |
+-----------+------------+----------------------------------------------------------------------------------------------------+
| showstats | boolean    | *Optional.* If *true*, a summary of user's stats will be returned.                                 |
+-----------+------------+----------------------------------------------------------------------------------------------------+
| listtype  | string     | *Optional.* A enum with 3 possible values. *active*, *inactive*, *moderate*. If *active*,          |
|           |            | users with activity status as only "active" are returned.                                          |
+-----------+------------+----------------------------------------------------------------------------------------------------+

.. |authentication| raw:: html

   <a href="api-rest-auth.html" target="_blank">authentication</a>

**RESPONSE FORMAT**

On error, the header status code is an error code and the response body contains an |error object|.

.. |error object| raw:: html

   <a href="api-documentation-plugin-rest-response-status-code.html#error-response-body" target="_blank">error object</a>

On success, the HTTP status code in the response header is 200 OK and the response body contains a list of user object.

**User object**

+-----------------------+------------+--------------------------------------------------------------------------------+
| Parameter             | Value Type | Description                                                                    |
+=======================+============+================================================================================+
| user_id               | integer    | Unique id of the user                                                          |
+-----------------------+------------+--------------------------------------------------------------------------------+
| user_name             | string     | Full name of the user                                                          |
+-----------------------+------------+--------------------------------------------------------------------------------+
| user_email            | string     | Email-id of the user                                                           |
+-----------------------+------------+--------------------------------------------------------------------------------+
| user_desg             | string     | Designation of the user                                                        |
+-----------------------+------------+--------------------------------------------------------------------------------+
| user_image_url        | string     | Profile image url of the user                                                  |
+-----------------------+------------+--------------------------------------------------------------------------------+
| channel_name          | string     | Unique name of account where user is registered in                             |
+-----------------------+------------+--------------------------------------------------------------------------------+
| app_name              | string     | Display name of the account where user is registered in                        |
+-----------------------+------------+--------------------------------------------------------------------------------+
| user_reg_date         | string     | Registration date of the user in 'yyyy-MM-dd hh:mm:ss' format in UTC time-zone |
+-----------------------+------------+--------------------------------------------------------------------------------+
| external_idp          | boolean    | *true* if user account is connected to a third-party identity provider         |
+-----------------------+------------+--------------------------------------------------------------------------------+
| external_idp_response | string     | Response recieved from the third-party identity provider                       |
+-----------------------+------------+--------------------------------------------------------------------------------+
| is_admin              | boolean    | *true* if the user is an admin                                                 |
+-----------------------+------------+--------------------------------------------------------------------------------+
| user_group            | array      | Array of groups which are assigned to the user                                 |
+-----------------------+------------+--------------------------------------------------------------------------------+

.. _get-user:

Get user
---------

This API returns a user object for the provided userId or Email within  the |scope| of the *tokenkey.*

**ENDPOINT**

.. code-block:: text 

	GET /users/{userIdORuserEmail}

**REQUEST PARAMETERS**

**Url segment**

+-------------------+-------------+------------------------------------------------------------------------------------------------+
| Parameter         | Value Type  | Description                                                                                    |
+===================+=============+================================================================================================+
| userIdORuserEmail | string      | *Required.* The "user id" or "email id" of the user whose profile information you which to     |
|                   |             | retrieve                                                                                       |
+-------------------+-------------+------------------------------------------------------------------------------------------------+

**Query string**

+-----------+------------+----------------------------------------------------------------------------------------------------+
| Parameter | Value Type | Description                                                                                        |
+===========+============+====================================================================================================+
| tokenkey  | string     | *Required.* Your API key. Check |authentication| for more details                                  |
+-----------+------------+----------------------------------------------------------------------------------------------------+
| nonce     | string     | *Optional.* Recieved as SSO parameter from the container app via the dronahq.js                    |
+-----------+------------+----------------------------------------------------------------------------------------------------+
| stats     | boolean    | *Optional.* If *true*, a summary of user's CMS activity will be returned in the response object.   |
+-----------+------------+----------------------------------------------------------------------------------------------------+

**RESPONSE FORMAT**

On error, the header status code is an error code and the response body contains an |error object|

On success, the HTTP status code in the response header is 200 OK and the response body contains user object.

**User object**

+-----------------------+------------+--------------------------------------------------------------------------------+
| Parameter             | Value Type | Description                                                                    |
+=======================+============+================================================================================+
| user_id               | integer    | Unique id of the user                                                          |
+-----------------------+------------+--------------------------------------------------------------------------------+
| user_name             | string     | Full name of the user                                                          |
+-----------------------+------------+--------------------------------------------------------------------------------+
| user_email            | string     | Email-id of the user                                                           |
+-----------------------+------------+--------------------------------------------------------------------------------+
| user_desg             | string     | Designation of the user                                                        |
+-----------------------+------------+--------------------------------------------------------------------------------+
| user_image_url        | string     | Profile image url of the user                                                  |
+-----------------------+------------+--------------------------------------------------------------------------------+
| channel_name          | string     | Unique name of account where user is registered in                             |
+-----------------------+------------+--------------------------------------------------------------------------------+
| app_name              | string     | Display name of the account where user is registered in                        |
+-----------------------+------------+--------------------------------------------------------------------------------+
| user_reg_date         | string     | Registration date of the user in 'yyyy-MM-dd hh:mm:ss' format in UTC time-zone |
+-----------------------+------------+--------------------------------------------------------------------------------+
| external_idp          | boolean    | *true* if user account is connected to a third-party identity provider         |
+-----------------------+------------+--------------------------------------------------------------------------------+
| external_idp_response | string     | Response recieved from the third-party identity provider                       |
+-----------------------+------------+--------------------------------------------------------------------------------+
| is_admin              | boolean    | *true* if the user is an admin                                                 |
+-----------------------+------------+--------------------------------------------------------------------------------+
| user_group            | array      | Array of groups which are assigned to the user                                 |
+-----------------------+------------+--------------------------------------------------------------------------------+

.. _create-users:

Create users
---------------

This API creates a user account with pre-registered password and group(s) based on the |scope| of the *tokenkey.*

**ENDPOINT**

.. code-block:: text 

	POST /users

**REQUEST PARAMETERS**

**Request body data**

+-------------------+-------------+------------------------------------------------------------------------------------------------+
| Parameter         | Value Type  | Description                                                                                    |
+===================+=============+================================================================================================+
| token_key         | string      | *Required.* Your API key. Check |authentication| for more details                              |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| pre_register      | bool        | *Required.* Set value to *true*                                                                |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| invitee_user      | array       | *Required.* An array of the invitee object. A maximum of 50 users can be sent in one request.  | 
+-------------------+-------------+------------------------------------------------------------------------------------------------+

**Invitee object data**

+-------------------+-------------+---------------------------------------------------+
| Parameter         | Value Type  | Description                                       |
+===================+=============+===================================================+
| user_name         | string      | *Required.* Full name of the invitee              |
+-------------------+-------------+---------------------------------------------------+
| user_email        | string      | *Required.* Email-id of the invitee               |
+-------------------+-------------+---------------------------------------------------+
| user_group_name   | array       | *Required.* A string array of the group-names     |
+-------------------+-------------+---------------------------------------------------+
| user_password     | string      | *Required.* Password for the invitee              |
+-------------------+-------------+---------------------------------------------------+

**RESPONSE FORMAT**

On error, the header status code is an error code and the response body contains an |error object|

On success, the HTTP status code in the response header is 200 OK and the response body contains an empty array.

However, even when the HTTP status code in the response header is 200 OK, pre-registration of few/all invitee might fail.

In such cases the response body would contain an array of the *error object of an invitee* whose registration failed.

**Error object of an invitee**

+-----------------+------------+-----------------------------------+
| Parameter       | Value Type | Description                       |
+=================+============+===================================+
| user_email      | string     | Email-id of the invitee           |
+-----------------+------------+-----------------------------------+
| error_code      | string     | Error code                        |
+-----------------+------------+-----------------------------------+
| error_detail    | string     | Detailed description of the error |
+-----------------+------------+-----------------------------------+

**Possible error codes**

+------+-------------------------------------------------------------------------------------------------+
| Code | Description                                                                                     |
+======+=================================================================================================+
| 2    | Email-id is not in correct format.                                                              |
+------+-------------------------------------------------------------------------------------------------+
| 3    | User already exists in the account.                                                             |
+------+-------------------------------------------------------------------------------------------------+
| 4    | The user was registered to atleast one of the mentioned groups but failed for a few.            |
+------+-------------------------------------------------------------------------------------------------+
| 5    | The user could not be registered to any of the mentioned groups. In this case user will not be  |
|      | added to the channel.                                                                           |
+------+-------------------------------------------------------------------------------------------------+
| 6    | User license expired. Contact our support desk for more detail.                                 |
+------+-------------------------------------------------------------------------------------------------+

.. _assign-grp-user:

Assign group(s) to a user
--------------------------

This API can be used to assign a list of groups to a user and can also be used to removes a list of groups assigned to a  user within the |scope| of its *tokenkey*.

**ENDPOINT**

.. code-block:: text 

	PUT /users/{userId}/actions/change_group

**REQUEST PARAMETER**

**Url segment**

+-------------------+-------------+------------------------------------------------------------------------------------------------+
| Parameter         | Value Type  | Description                                                                                    |
+===================+=============+================================================================================================+
| userId            | integer     | *Required.* The unique id of the user.                                                         |
+-------------------+-------------+------------------------------------------------------------------------------------------------+

**Request body data**

+-------------------+-------------+------------------------------------------------------------------------------------------------+
| Parameter         | Value Type  | Description                                                                                    |
+===================+=============+================================================================================================+
| token_key         | string      | *Required.* Your API key. Check |authentication| for more details                              |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| assign_group      | array       | *Optional.* A string array of unique names of groups to be assigned to the user.               |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| remove_group      | array       |  Optional. A string array of unique names of groups to be removed from the user. 		   |
| 		    |		  |  This parameter also accepts '*' character as a wildcard to remove all groups.                 | 
+-------------------+-------------+------------------------------------------------------------------------------------------------+

Please note that either assign_group or remove_group should contain at least one **group name** in the request body.

**RESPONSE FORMAT**

On error, the header status code is an error code and the response body contains an |error object|. On success, the HTTP status code in the response header is 200 OK and the response body contains a JSON object with a list of groups successfully *assigned or removed for* a user.

-Please note that even when the HTTP status code in the response header is 200 OK,  *assigning or removing* groups for a user might fail for other reasons such as the group is not a valid group, the API failed to perform the operation. In such cases, the response body would contain an array of the invalid groups in the invalid_groups  field and an array of groups for whom the API operation failed in the failed_groups field.

**Success response data**

+-------------------+-------------+------------------------------------------------------------------------------------------------+
| Parameter         | Value Type  | Description                                                                                    |
+===================+=============+================================================================================================+
| user_id           | integer     | The unique id of the user.                                                                     |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| assigned_to       | array       | An integer array of unique group ids to which the user was successfully assigned.              |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| removed_from      | array       | An integer array of unique group ids from where the user was successfully removed.             | 
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| invalid_groups    | array       | An integer array of unique group ids that are not valid.                                       |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| failed_groups     | array       | An integer array of unique id of groups on which the operation of assigning/removing failed.   | 
|                   |             | Retry again with these groups, if problem persists contact our support desk.                   | 
+-------------------+-------------+------------------------------------------------------------------------------------------------+

.. _activate-users:

Activate Users
----------------
This API activates registered users based on the |scope| of the *tokenkey.*

**ENDPOINT**

.. code-block:: text 

	PUT /users

**REQUEST PARAMETER**

**Request Body Data**

+-------------------+-------------+------------------------------------------------------------------------------------------------+
| Parameter         | Value Type  | Description                                                                                    |
+===================+=============+================================================================================================+
| token_key         | string      | *Required.* Your API key. Check |authentication| for more details                              |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| list_user_email   | array       | *Required.* A string array of the users email. A maximum of 50 IDs can be sent in one request. |
+-------------------+-------------+------------------------------------------------------------------------------------------------+

**RESPONSE FORMAT**

On error, the header status code is an error code and the response body contains an |error object|. On success, the HTTP status code in the response header is 200 OK and the response body contains a JSON object.

**Success Response Data**

+-------------------+-------------+------------------------------------------------------------------------------------------------+
| Parameter         | Value Type  | Description                                                                                    |
+===================+=============+================================================================================================+
| users_success     | integer     | A string array of user email whose account has been successfully activated.                    |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| users_invalid     | array       | A string array of invalid user email.                                                          |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| user_not_exist    | array       | A string array of user not found in the API key scope.                                         |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| users_failed	    | array       | A string array of user email on which the operation of activation failed.                      |
+-------------------+-------------+------------------------------------------------------------------------------------------------+

.. _deactivate-users:

Deactivate Users
-----------------

This API deactivates users based on the |scope| of the *tokenkey.*

**ENDPOINT**

.. code-block:: text 

	DELETE /users

**REQUEST PARAMETER**

**Request Body Data**

+-------------------+-------------+------------------------------------------------------------------------------------------------+
| Parameter         | Value Type  | Description                                                                                    |
+===================+=============+================================================================================================+
| token_key         | string      | *Required.* Your API key. Check |authentication| for more details                              |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| list_user_email   | array       | *Required.* A string array of the users email. A maximum of 50 IDs can be sent in one request. |
+-------------------+-------------+------------------------------------------------------------------------------------------------+

**RESPONSE FORMAT**

On error, the header status code is an error code and the response body contains an |error object|. On success, the HTTP status code in the response header is 200 OK and the response body contains a JSON object.

**Success Response Data**

+-------------------+-------------+------------------------------------------------------------------------------------------------+
| Parameter         | Value Type  | Description                                                                                    |
+===================+=============+================================================================================================+
| users_success     | integer     | A string array of user email whose account has been successfully deactivated.                  |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| users_invalid     | array       | A string array of invalid user email.                                                          |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| user_not_exist    | array       | A string array of user not found in the API key scope.                                         |
+-------------------+-------------+------------------------------------------------------------------------------------------------+
| users_failed      | array       | A string array of user email on which the operation of deactivation failed.                    |
+-------------------+-------------+------------------------------------------------------------------------------------------------+

