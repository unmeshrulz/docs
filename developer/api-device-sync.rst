.. _ref-device-sync:

Sync
====
This family of device APIs provided by dronahq.js allows a micro-app an easy way to submit data in background via http requests. 

It has the following method(s) -

	- :ref:`upload() <upload>`
	- :ref:`refresh() <refresh>`
	- :ref:`getpendinguploadcount() <getpendinguploadcount>`

It also provide the following event(s) -

	- :ref:`uploadcomplete <uploadcomplete>`

.. _upload:

upload()
--------

This method uploads JSON data to a remote URL.  Optionally you can also upload an image file using this request. The JSON data is sent in the request body. By invoking this method, the micro-app will keep trying the data upload until the server returns an http 2XX response. For failed requests, the app will keep trying to upload the data in every 1 hour.

Please note that calling this method only submits the request to the Container App. Actual upload happens when the sync service is triggered the next time. You can call the refresh method to trigger the sync service.


**REQUEST PARAMETERS**

+--------------+----------+-----------------------------------------+
|Parameter     |Type      |Description                              |
+==============+==========+=========================================+
|remoteurl     |string    |Required. http(s) URL of your API where  |
|              |          |the data should be submitted             |
+--------------+----------+-----------------------------------------+
|requestmethod |string    |Required. HTTP method type for the       |
|              |          |request. Supported values are            |
|              |          |'POST' & 'PUT'                           |
+--------------+----------+-----------------------------------------+
|requestdata   |object    |*Required* A valid JSON object which will|
|              |          |be sent in the request body.             |
+--------------+----------+-----------------------------------------+
|imagefilepath |string    |*Optional* File Path URI for the image   |
|              |          |which should also get uploaded along with|
|              |          |the request.                             |
+--------------+----------+-----------------------------------------+
|options       |object    |*Optional* A object with more request    |
|              |          |parameters like header & timeout.        |
+--------------+----------+-----------------------------------------+
|fnsuccess     |function  |*Optional* Callback function which will  |
|              |          |be called after request has been added to|
|              |          |the queue.                               |
+--------------+----------+-----------------------------------------+
|fnerror       |function  |*Optional* Callback function which will  |
|              |          |be called when request could not be added|
|              |          |to the queue.                            |
+--------------+----------+-----------------------------------------+

.. code:: javascript

	var remUrl = 'https://yourserver.com/api/users/134/action/updatedesignation';
	var dronaHQReqMethod = 'POST';
	var dronaHQReqData = {designation_code : 'X601'};
	var reqImgURI = '';
	var headers = [{ Authorization: 'Auth a2FuY2hhbkBkcm9uYW1vYmlsZS5jb206bWFpbEAxMjM0' }];

	var options = {};
	options.header = headers;
            
	DronaHQ.sync.upload(remUrl, dronaHQReqMethod, dronaHQReqData, reqImgURI, options);

.. _refresh:

refresh()
---------

Calling this method will wake up the sync service and triggers any pending upload.

.. code:: javascript
	
	//refreshType = 'upload' for submitting any pending uploads

	DronaHQ.sync.refresh(refreshType);
	
.. _getpendinguploadcount:

getpendinguploadcount()
------------------------

This method gives a count of all the pending upload requests.

.. code:: javascript

	DronaHQ.sync.getPendingUploadCount(function(count){}, function(e){});

.. _uploadcomplete:

uploadcomplete
--------------

.. code:: javascript

	DronaHQ.sync.uploadcomplete 


This event is triggered whenever the sync service has finished processing all pending requests, even if a few requests have failed to complete successfully. The failed requests are retried next time the service runs.

.. code:: javascript
	
	document.addEventListener('dronahq.sync.uploadcomplete', function(){
		//Refresh task is complete.
	});








