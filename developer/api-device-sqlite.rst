.. _ref-device-sqlite:

SQlite storage adapter
======================

Interface to sqlite in a dronahq microapp, with API similar to HTML5/|Web SQL API|.

Status
-------

	- SQLite version 3.8.10.2 is supported for all supported platforms Android/iOS/Windows Phone.
	- In case of memory issues please use smaller transactions.

Highlights
-----------

	- Drop-in replacement for HTML5/|Web SQL API|: the only change should be to replace the static window.openDatabase()factory call with window.sqlitePlugin.openDatabase(), with parameters as documented below.
	- Failure-safe nested transactions with batch processing optimizations (according to HTML5/|Web SQL API|)
	- As described in |this posting|:
	
		- No 5MB maximum, more information at: http://www.sqlite.org/limits.html

.. |this posting| raw:: html

   <a href="http://brodyspark.blogspot.com/2012/12/cordovaphonegap-sqlite-plugins-offer.html" target="_blank">this posting</a>
   
Usage
------

General
~~~~~~~~~~

	- Drop-in replacement for HTML5/|Web SQL API|: the only change should be to replace the static window.openDatabase()factory call with window.sqlitePlugin.openDatabase(), with parameters as documented below. Some other known deviations are documented below. Please report if you find any other possible deviations.

NOTE: If a sqlite statement in a transaction fails with an error, the error handler must return false in order to recover the transaction. This is correct according to the HTML5/|Web SQL API| standard. This is different from the WebKit implementation of Web SQL in Android and iOS which recovers the transaction if a sql error hander returns a non-true value.

.. |Web SQL API| raw:: html

   <a href="http://www.w3.org/TR/webdatabase/" target="_blank">Web SQL API</a>

For some basic examples, see the |Sample section|

.. |Sample section| raw:: html

   <a href="https://github.com/dronahq/documentation/blob/master/device-api/sqlite-storage.md#sample" target="_blank">Sample section</a>
   
Opening a database
~~~~~~~~~~~~~~~~~~

To open a database access handle object:

var db = window.sqlitePlugin.openDatabase({name: 'my.db'}, successcb, errorcb);

IMPORTANT: Please wait for the 'deviceready' event, as in the following example:

.. code:: javascript

	// Wait for dronahq to load
	document.addEventListener('deviceready', onDeviceReady, false);
	// DronaHQ is ready
	function onDeviceReady() {
		var db = window.sqlitePlugin.openDatabase({name: 'my.db'});
		// ...
	}

The successcb and errorcb callback parameters are optional but can be extremely helpful in case anything goes wrong. For example:

.. code:: javascript

	window.sqlitePlugin.openDatabase({name: 'my.db'}, function(db) {
		db.transaction(function(tx) {
			// ...
		}, function(err) {
			console.log('Open database ERROR: ' + JSON.stringify(err));
		});
	});

If any sql statements or transactions are attempted on a database object before the openDatabase result is known, they will be queued and will be aborted in case the database cannot be opened.
OTHER NOTES:

	- It is possible to open multiple database access handle objects for the same database.
	- The database handle access object can be closed as described below.

Web SQL replacement tip:
To overwrite window.openDatabase:

.. code:: javascript

	window.openDatabase = function(dbname, ignored1, ignored2, ignored3) {
		return window.sqlitePlugin.openDatabase({name: dbname});
	};

SQL transactions
~~~~~~~~~~~~~~~~
The following types of SQL transactions are supported by this version:

	- Single-statement transactions
	- SQL batch query transactions
	- Standard asynchronous transactions

**Single-statement transactions**

Sample with INSERT:

.. code:: javascript

	db.executeSql('INSERT INTO MyTable VALUES (?)', ['test-value'], function (resultSet) {
		console.log('resultSet.insertId: ' + resultSet.insertId);
		console.log('resultSet.rowsAffected: ' + resultSet.rowsAffected);
	}, function(error) {
		console.log('SELECT error: ' + error.message);
	});

Sample with SELECT:

.. code:: javascript

	db.executeSql("SELECT LENGTH('tenletters') AS stringlength", [], function (resultSet) {
		console.log('got stringlength: ' + resultSet.rows.item(0).stringlength);
	}, function(error) {
		console.log('SELECT error: ' + error.message);
	});

NOTE/minor bug: The object returned by resultSet.rows.item(rowNumber) is not immutable. In addition, multiple calls toresultSet.rows.item(rowNumber) with the same rowNumber on the same resultSet object return the same object. For example, the following code will show Second uppertext result: ANOTHER:

.. code:: javascript

	db.executeSql("SELECT UPPER('First') AS uppertext", [], function (resultSet) {
		var obj1 = resultSet.rows.item(0);
		obj1.uppertext = 'ANOTHER';
		console.log('Second uppertext result: ' + resultSet.rows.item(0).uppertext);
		console.log('SELECT error: ' + error.message);
	});

**SQL batch query transactions**

Sample:

.. code:: javascript

	db.sqlBatch([
		'DROP TABLE IF EXISTS MyTable',
		'CREATE TABLE MyTable (SampleColumn)',
		[ 'INSERT INTO MyTable VALUES (?)', ['test-value'] ],
	], function() {
		db.executeSql('SELECT * FROM MyTable', [], function (resultSet) {
			console.log('Sample column value: ' + resultSet.rows.item(0).SampleColumn);
		});
	}, function(error) {
		console.log('Populate table error: ' + error.message);
	});

In case of an error, all changes in a sql batch are automatically discarded using ROLLBACK.

**Standard asynchronous transactions**

Standard asynchronous transactions follow the HTML5/|Web SQL API| which is very well documented and uses BEGIN and COMMIT or ROLLBACK to keep the transactions failure-safe. Here is a simple example:
   
.. code:: javascript

	db.transaction(function(tx) {
		tx.executeSql('DROP TABLE IF EXISTS MyTable');
		tx.executeSql('CREATE TABLE MyTable (SampleColumn)');
		tx.executeSql('INSERT INTO MyTable VALUES (?)', ['test-value'], function(tx, resultSet) {
			console.log('resultSet.insertId: ' + resultSet.insertId);
			console.log('resultSet.rowsAffected: ' + resultSet.rowsAffected);
		}, function(tx, error) {
			console.log('INSERT error: ' + error.message);
		});
	}, function(error) {
		console.log('transaction error: ' + error.message);
	}, function() {
		console.log('transaction ok');
	});

In case of a read-only transaction, it is possible to use readTransaction which will not use BEGIN, COMMIT, or ROLLBACK:

.. code:: javascript

	db.readTransaction(function(tx) {
		tx.executeSql("SELECT UPPER('Some US-ASCII text') AS uppertext", [], function(tx, resultSet) {
			console.log("resultSet.rows.item(0).uppertext: " + resultSet.rows.item(0).uppertext);
		}, function(tx, error) {
			console.log('SELECT error: ' + error.message);
		});
	}, function(error) {
		console.log('transaction error: ' + error.message);
	}, function() {
		console.log('transaction ok');
	});

WARNING: It is NOT allowed to execute sql statements on a transaction after it has finished. Here is an example from thePopulating Cordova SQLite storage with the JQuery API post at |http://www.brodybits.com/cordova/sqlite/api/jquery/2015/10/26/populating-cordova-sqlite-storage-with-the-jquery-api.html|:

.. |http://www.brodybits.com/cordova/sqlite/api/jquery/2015/10/26/populating-cordova-sqlite-storage-with-the-jquery-api.html| raw:: html

   <a href="http://www.brodybits.com/cordova/sqlite/api/jquery/2015/10/26/populating-cordova-sqlite-storage-with-the-jquery-api.html" target="_blank">http://www.brodybits.com/cordova/sqlite/api/jquery/2015/10/26/populating-cordova-sqlite-storage-with-the-jquery-api.html</a>

.. code:: javascript
   
	// BROKEN SAMPLE:
	var db = window.sqlitePlugin.openDatabase({name: "test.db"});
	db.executeSql("DROP TABLE IF EXISTS tt");
	db.executeSql("CREATE TABLE tt (data)");

	db.transaction(function(tx) {
		$.ajax({
			url: 'https://api.github.com/users/litehelpers/repos',
			dataType: 'json',
			success: function(res) {
				console.log('Got AJAX response: ' + JSON.stringify(res));
				$.each(res, function(i, item) {
					console.log('REPO NAME: ' + item.name);
					tx.executeSql("INSERT INTO tt values (?)", JSON.stringify(item.name));
				});
			}
		});
	}, function(e) {
		console.log('Transaction error: ' + e.message);
	}, function() {
		// Check results:
		db.executeSql('SELECT COUNT(*) FROM tt', [], function(res) {
			console.log('Check SELECT result: ' + JSON.stringify(res.rows.item(0)));
		});
	});

You can find more details and a step-by-step description how to do this right in the Populating Cordova SQLite storage with the JQuery API post at: |http://www.brodybits.com/cordova/sqlite/api/jquery/2015/10/26/populating-cordova-sqlite-storage-with-the-jquery-api.html|
   
NOTE/minor bug: Just like the single-statement transaction described above, the object returned byresultSet.rows.item(rowNumber) is not immutable. In addition, multiple calls to resultSet.rows.item(rowNumber) with the same rowNumber on the same resultSet object return the same object. For example, the following code will show Second uppertext result: ANOTHER:

.. code:: javascript

	db.readTransaction(function(tx) {
		tx.executeSql("SELECT UPPER('First') AS uppertext", [], function(tx, resultSet) {
			var obj1 = resultSet.rows.item(0);
			obj1.uppertext = 'ANOTHER';
			console.log('Second uppertext result: ' + resultSet.rows.item(0).uppertext);
			console.log('SELECT error: ' + error.message);
		});
	});

FUTURE TBD: It should be possible to get a row result object using resultSet.rows[rowNumber], also in case of a single-statement transaction. This is non-standard but is supported by the Chrome desktop browser.

Background processing
~~~~~~~~~~~~~~~~~~~~~

The threading model depends on which version is used:

	- For Android, one background thread per db;
	- For iOS, background processing using a very limited thread pool (only one thread working at a time);
	- For Windows, no background processing.

Sample with PRAGMA feature
~~~~~~~~~~~~~~~~~~~~~~~~~~

Creates a table, adds a single entry, then queries the count to check if the item was inserted as expected. Note that a new transaction is created in the middle of the first callback.

.. code:: javascript

	// Wait for DronaHQ to load
	document.addEventListener('deviceready', onDeviceReady, false);
	// DronaHQ is ready
	function onDeviceReady() {
		var db = window.sqlitePlugin.openDatabase({name: 'my.db'});
		db.transaction(function(tx) {
			tx.executeSql('DROP TABLE IF EXISTS test_table');
			tx.executeSql('CREATE TABLE IF NOT EXISTS test_table (id integer primary key, data text, data_num integer)');
			// demonstrate PRAGMA:
			db.executeSql("pragma table_info (test_table);", [], function(res) {
				console.log("PRAGMA res: " + JSON.stringify(res));
			});

			tx.executeSql("INSERT INTO test_table (data, data_num) VALUES (?,?)", ["test", 100], function(tx, res) {
				console.log("insertId: " + res.insertId + " -- probably 1");
				console.log("rowsAffected: " + res.rowsAffected + " -- should be 1");

				db.transaction(function(tx) {
					tx.executeSql("select count(id) as cnt from test_table;", [], function(tx, res) {
						console.log("res.rows.length: " + res.rows.length + " -- should be 1");
						console.log("res.rows.item(0).cnt: " + res.rows.item(0).cnt + " -- should be 1");
					});
				});
			}, function(e) {
				console.log("ERROR: " + e.message);
			});
		});
	}

NOTE: PRAGMA statements must be executed in executeSql() on the database object (i.e. db.executeSql()) and NOT within a transaction.

Sample with transaction-level nesting
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In this case, the same transaction in the first executeSql() callback is being reused to run executeSql() again.

.. code:: javascript

	// Wait for DronaHQ to load
	document.addEventListener('deviceready', onDeviceReady, false);
	// DronaHQ is ready
	function onDeviceReady() {		
		var db = window.sqlitePlugin.openDatabase({name: 'my.db'});
		db.transaction(function(tx) {			
			tx.executeSql('DROP TABLE IF EXISTS test_table');
			tx.executeSql('CREATE TABLE IF NOT EXISTS test_table (id integer primary key, data text, data_num integer)');
			tx.executeSql("INSERT INTO test_table (data, data_num) VALUES (?,?)", ["test", 100], function(tx, res) {
				console.log("insertId: " + res.insertId + " -- probably 1");
				console.log("rowsAffected: " + res.rowsAffected + " -- should be 1");
				tx.executeSql("select count(id) as cnt from test_table;", [], function(tx, res) {
					console.log("res.rows.length: " + res.rows.length + " -- should be 1");
					console.log("res.rows.item(0).cnt: " + res.rows.item(0).cnt + " -- should be 1");
				});					
			}, function(tx, e) {
				console.log("ERROR: " + e.message);
			});
		});
	}

This case will also works with Safari (WebKit), assuming you replace window.sqlitePlugin.openDatabase withwindow.openDatabase.

Close a database object
~~~~~~~~~~~~~~~~~~~~~~~~~

This will invalidate all handle access handle objects for the database that is closed:

.. code:: javascript

	db.close(successcb, errorcb);

It is OK to close the database within a transaction callback but NOT within a statement callback. The following example is OK:

.. code:: javascript

	db.transaction(function(tx) {
		tx.executeSql("SELECT LENGTH('tenletters') AS stringlength", [], function(tx, res) {
			console.log('got stringlength: ' + res.rows.item(0).stringlength);
		});
	}, function(error) {
		// OK to close here:
		console.log('transaction error: ' + error.message);
		db.close();
	}, function() {
		// OK to close here:
		console.log('transaction ok');
		db.close(function() {
			console.log('database is closed ok');
		});
	});

The following example is NOT OK:

.. code:: javascript

	// BROKEN:db.transaction(function(tx) {
		tx.executeSql("SELECT LENGTH('tenletters') AS stringlength", [], function(tx, res) {
			console.log('got stringlength: ' + res.rows.item(0).stringlength);
			// BROKEN - this will trigger the error callback:
			db.close(function() {
				console.log('database is closed ok');
			}, function(error) {
			console.log('ERROR closing database');
			});
		});
	});

BUG: It is currently NOT possible to close a database in a db.executeSql callback. For example:

.. code:: javascript

	// BROKEN DUE TO BUG:db.executeSql("SELECT LENGTH('tenletters') AS stringlength", [], function (res) {
		var stringlength = res.rows.item(0).stringlength;
		console.log('got stringlength: ' + res.rows.item(0).stringlength);

		// BROKEN - this will trigger the error callback DUE TO BUG:
		db.close(function() {
			console.log('database is closed ok');
		}, function(error) {
			console.log('ERROR closing database');
		});
	});

SECOND BUG: When a database connection is closed, any queued transactions are left hanging. All pending transactions should be errored when a database connection is closed.
NOTE: As described above, if multiple database access handle objects are opened for the same database and one database handle access object is closed, the database is no longer available for the other database handle objects. Possible workarounds:

	- It is still possible to open one or more new database handle objects on a database that has been closed.
	- It should be OK not to explicitly close a database handle since database transactions are |ACID| compliant and the app's memory resources are cleaned up by the system upon termination.

.. |ACID| raw:: html
	
	<a href=" https://en.wikipedia.org/wiki/ACID" target="_blank">ACID</a>

FUTURE TBD: dispose method on the database access handle object, such that a database is closed once all access handle objects are disposed.

Delete a database
~~~~~~~~~~~~~~~~~~~~

.. code:: javascript
	
	window.sqlitePlugin.deleteDatabase({name: 'my.db'}, successcb, errorcb);

BUG: When a database is deleted, any queued transactions for that database are left hanging. All pending transactions should be errored when a database is deleted.

Database schema versions
------------------------

The transactional nature of the API makes it relatively straightforward to manage a database schema that may be upgraded over time (adding new columns or new tables, for example).
Here is the recommended procedure to follow upon app startup:

	- Check your database schema version number (you can use db.executeSql since it should be a very simple query)
	- If your database needs to be upgraded, do the following within a single transaction to be failure-safe:
	
		- Create your database schema version table (single row single column) if it does not exist (you can check thesqlite_master table as described at: |http://stackoverflow.com/questions/1601151/how-do-i-check-in-sqlite-whether-a-table-exists|)
		- Add any missing columns and tables, and apply any other changes necessary

IMPORTANT: Since we cannot be certain when the users will actually update their apps, old schema versions will have to be supported for a very long time.

.. |http://stackoverflow.com/questions/1601151/how-do-i-check-in-sqlite-whether-a-table-exists| raw:: html
	
	<a href="http://stackoverflow.com/questions/1601151/how-do-i-check-in-sqlite-whether-a-table-exists" target="_blank">http://stackoverflow.com/questions/1601151/how-do-i-check-in-sqlite-whether-a-table-exists</a>
	
Use with Ionic/ngCordova/Angular
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is recommended to follow the tutorial at: |https://blog.nraboy.com/2014/11/use-sqlite-instead-local-storage-ionic-framework/|

.. |https://blog.nraboy.com/2014/11/use-sqlite-instead-local-storage-ionic-framework/| raw:: html
	
	<a href="https://blog.nraboy.com/2014/11/use-sqlite-instead-local-storage-ionic-framework/" target="_blank">https://blog.nraboy.com/2014/11/use-sqlite-instead-local-storage-ionic-framework/</a>
	
A sample is provided at: litehelpers/Ionic-sqlite-database-example

.. |litehelpers/Ionic-sqlite-database-example| raw:: html
	
	<a href="https://github.com/litehelpers/Ionic-sqlite-database-example" target="_blank">litehelpers/Ionic-sqlite-database-example</a>
	
Documentation at: |http://ngcordova.com/docs/plugins/sqlite/|

.. |http://ngcordova.com/docs/plugins/sqlite/| raw:: html
	
	<a href="http://ngcordova.com/docs/plugins/sqlite/" target="_blank">http://ngcordova.com/docs/plugins/sqlite/</a>
	
Some known deviations from the Web SQL database standard
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

	- The window.sqlitePlugin.openDatabase static factory call takes a different set of parameters than the standard Web SQLwindow.openDatabase static factory call. In case you have to use existing Web SQL code with no modifications please see the Web SQL replacement tip below.
	- This plugin does not support the database creation callback or standard database versions. Please read the Database schema versions section below for tips on how to support database schema versioning.
	- This plugin does not support the synchronous Web SQL interfaces.
	- Error reporting is not 100% compliant, with some issues described below.
	- In case of a transaction with an sql statement error for which there is no error handler, the error handler does not returnfalse, or the error handler throws an exception, the plugin will fire more sql statement callbacks before the transaction is aborted with ROLLBACK.
	- Known issues with handling of certain ASCII/UNICODE characters as described below.
	- This plugin supports some non-standard features as described below.

Known issues
~~~~~~~~~~~~~~

	- iOS version does not support certain rapidly repeated open-and-close or open-and-delete test scenarios due to how the implementation handles background processing
	- As described below, auto-vacuum is NOT enabled by default.
	- Memory issue observed when adding a large number of records due to the JSON implementation.
	- A stability issue was reported on the iOS version when in use together with |SockJS| client such as |pusher-js| at the same time (see |litehelpers/Cordova-sqlite-storage#196|). The workaround is to call sqlite functions and |SockJS| client functions in separate ticks (using setTimeout with 0 timeout).
	- If a sql statement fails for which there is no error handler or the error handler does not return false to signal transaction recovery, the plugin fires the remaining sql callbacks before aborting the transaction.
	- In case of an error, the error code member is bogus on Android and Windows Phone.
	- Possible crash on Android when using Unicode emoji characters due to |Android bug 81341|, which should be fixed in Android 6.x
	- Close/delete database bugs described below.
	- When a database is opened and deleted without closing, the iOS version is known to leak resources.
	- Incorrect or missing insertId/rowsAffected in results for INSERT/UPDATE/DELETE SQL statements with extra semicolon(s) in the beginning for Android in case the androidDatabaseImplementation: 2 (built-in android.database implementation) option is used.
	- Within a readTransaction the plugin executes SQL write statements that start with extra semicolon(s).
	- Unlike the HTML5/|Web SQL API| this plugin handles executeSql calls with too few parameters without error reporting and the iOS version handles executeSql calls with too many parameters without error reporting.

.. |SockJS| raw:: html

   <a href="http://sockjs.org/" target="_blank">SockJS</a>

.. |pusher-js| raw:: html

   <a href="https://github.com/pusher/pusher-js" target="_blank">pusher-js</a>

.. |litehelpers/Cordova-sqlite-storage#196| raw:: html

   <a href="https://github.com/litehelpers/Cordova-sqlite-storage/issues/196" target="_blank">litehelpers/Cordova-sqlite-storage#196</a>
   
.. |Android bug 81341| raw:: html

   <a href="https://code.google.com/p/android/issues/detail?id=81341" target="_blank">Android bug 81341</a>
   
**Other limitations**

	- The db version, display name, and size parameter values are not supported and will be ignored.- (No longer supported by the API)
	- Absolute and relative subdirectory path(s) are not tested or supported.
	- This plugin will not work before the callback for the 'deviceready' event has been fired, as described in Usage.
	- This version will not work within a web worker (not properly supported by the Cordova framework).
	- In-memory database db=window.sqlitePlugin.openDatabase({name: ':memory:', ...}) is currently not supported.
	- The Android version cannot work with more than 100 open db files (due to the threading model used).
	- UNICODE \u2028 (line separator) and \u2029 (paragraph separator) characters are currently not supported and known to be broken in iOS version due to |Cordova bug CB-9435|. There may be a similar issue with certain other UNICODE characters in the iOS version (needs further investigation).
	- BLOB type is not supported in this version.
	- UNICODE \u0000 (same as \0) character not working in Android or Windows Phone
	- Case-insensitive matching and other string manipulations on Unicode characters, which is provided by optional ICU integration in the sqlite source and working with recent versions of Android, is not supported for any target platforms.
	- iOS version uses a thread pool but with only one thread working at a time due to "synchronized" database access
	- Large query result can be slow, also due to JSON implementation
	- ATTACH to another database file is not supported by this version.
	- UPDATE/DELETE with LIMIT or ORDER BY is not supported.
	- WITH clause is not supported by older Android versions in case the androidDatabaseImplementation: 2 (built-in android.database implementation) option is used.
	- User-defined savepoints are not supported and not expected to be compatible with the transaction locking mechanism used by this plugin. In addition, the use of BEGIN/COMMIT/ROLLBACK statements is not supported.
	- Problems have been reported when using this plugin with Crosswalk (for Android). It may help to install Crosswalk as a plugin instead of using Crosswalk to create the project.
	- Does not work with |axemclion/react-native-cordova-plugin| since the window.sqlitePlugin object is not properly exported (ES5 feature). It is recommended to use |andpor/react-native-sqlite-storage| for SQLite database access with React Native Android/iOS instead.

.. |Cordova bug CB-9435| raw:: html

   <a href="https://issues.apache.org/jira/browse/CB-9435" target="_blank">Cordova bug CB-9435</a>
   
.. |axemclion/react-native-cordova-plugin| raw:: html

   <a href="https://github.com/axemclion/react-native-cordova-plugin" target="_blank">axemclion/react-native-cordova-plugin</a>
    
.. |andpor/react-native-sqlite-storage| raw:: html

   <a href="https://github.com/andpor/react-native-sqlite-storage" target="_blank">andpor/react-native-sqlite-storage</a>
   
**Some tips and tricks**

	- If you run into problems and your code follows the asynchronous HTML5/|Web SQL| transaction API, you can try opening a test database using window.openDatabase and see if you get the same problems.
	- In case your database schema may change, it is recommended to keep a table with one row and one column to keep track of your own schema version number. It is possible to add it later. The recommended schema update procedure is described below.

.. |Web SQL| raw:: html

   <a href="http://www.w3.org/TR/webdatabase/" target="_blank">Web SQL</a>
   
**Common pitfall(s)**

	- It is NOT allowed to execute sql statements on a transaction that has already finished, as described below. This is consistent with the HTML5/Web SQL API (http://www.w3.org/TR/webdatabase/).
	- The plugin class name starts with "SQL" in capital letters, but in Javascript the sqlitePlugin object name starts with "sql" in small letters.
	- Attempting to open a database before receiving the 'deviceready' event callback.
	- Inserting STRING into ID field
	- Auto-vacuum is NOT enabled by default. It is recommended to periodically VACUUM the database.

**Angular/ngCordova/Ionic-related pitfalls**

	- Angular/ngCordova/Ionic controller/factory/service callbacks may be triggered before the 'deviceready' event is fired
	
Support
--------
**What information is needed for help**

Please include the following:
	- Which platform(s) Android/iOS/Windows Phone
	- Clear description of the issue
	- A small, complete, self-contained program that demonstrates the problem, preferably as a Gith

