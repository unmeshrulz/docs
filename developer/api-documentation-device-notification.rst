.. _ref-device-notification:

Notification
============

This family of device APIs provided by dronahq.js can be used to interact with the notification data received on the device sent by a micro-app. 
All notifications are received in the inbox of the container app. On clicking a notification the micro-app is invoked and the unique id of the notification is appended as query sting, which can be used to perform actions like fetching the relevant notification data.

It has the following method(s) -

	- :ref:`getanotification() <get-notification>`
	- :ref:`getallnotification() <get-all-notification>`
	
.. _get-notification:

getnotification()
-----------------

This method gets notification based on a notification id

.. code:: javascript

	//Get the notification id from query string

	function getQParameter(name) {
		name = name.replace(/[\[]/, "\\[").replace(/[\]]/, "\\]");
		var regex = new RegExp("[\\?&]" + name + "=([^&#]*)"),
		results = regex.exec(location.search);
		return results === null ? "" : decodeURIComponent(results[1].replace(/\+/g, " "));
	}
  
	var notiId = getQParameter('dm_noti_id');   

	var fnSuccess = function(notiData){
		//notiData contains the JSON object 
		//that was sent using platform notification API
		console.log(notiData);
    
	};
	var fnError = function(e){
		if(e.code == '1'){
			console.log('Invaid Notification ID');
		}
	};

	DronaHQ.notification.getNotification(notid, fnSuccess, fnError);

.. _get-all-notification:

getallnotification()
--------------------

This method gets all unread notification. Any notification which has not been retrieved by getNotification() is considered as unread.

.. code:: javascript

	var fnSuccess = function(data){
		var unreadNotiCount = data.notifications.length;
	};
	
	var fnError = function(e){};

	DronaHQ.notification.getAllNotification(fnSuccess, fnError);

