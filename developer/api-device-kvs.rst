.. _ref-device-kvs:

Key-value storage
=================

This family of device APIs allow micro-apps to leverage of the Container App's secure local storage API. DronaHQ Container App's secure local storage provides an alternative to the localStorage feature of HTML5. Any value assigned to a key must be of the type string.

Method(s)

	- :ref:`setItem() <set-item>`
	- :ref:`getItem() <get-item>`
	- :ref:`removeItem() <remove-item>`
	- :ref:`clear() <clear>`

.. _set-item:

Set Item
--------

This method allows micro-apps to set a string value to a key. 

.. code:: javascript

	var options = {};
	options.global = 1; // 1: Value can be read by all microapps. 0: Only to this microapp.
	
	DronaHQ.KVStore.setItem(keyName, keyValue, function() {
		//success callback
	}, function() {
		//fail callback
	}, options);

.. _get-item:

Get Item
---------

This method allows micro-apps to get the string value of a key. 

.. code:: javascript

	DronaHQ.KVStore.getItem(keyName, function(data) {
		//success callback
		var value = data.value;
	}, function() {
		//fail callback
	});

.. _remove-item:

Remove Item
------------

This method allows micro-apps to remove a key and its associated string value. 

.. code:: javascript

	DronaHQ.KVStore.removeItem(keyName, function() {
		//success callback
	}, function() {
		//fail callback
	});

.. _clear:

Clear All
----------

This method allows micro-apps to remove all key with their associated string value. 

.. code:: javascript
	
	DronaHQ.KVStore.clear(function() {
		//success callback
	}, function() {
		//fail callback
	});

