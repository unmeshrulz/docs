.. _quick-start:

Quick Start
===========

The purpose of Quick Start is to help you build a very simple HTML5 web application and deploy it inside the DronaHQ Client App container. This helps developers establish a basic foundation for developing and deploying apps on the DronaHQ platform.

As mentioned before, in the :ref:`DronaHQ SDK Overview <dronahq-sdk-overview>` guide, then DronaHQ platform's SDK consists of two major segments: 

	- Device APIs
	- REST APIs. 

While DronaHQ Rest APIs are to be consumed via HTTP requests,  using the Device APIs requires your application to include dronahq.js in the header section of your HTML page.

.. code:: html
	
	<script src="js/dronahq.js"></script>
	
You can get the latest copy of dronahq.js from our `GitHub repository`_ , and place it anywhere in your application's resources. 

.. _GitHub repository: https://github.com/dronahq

The following properties allow you to detect what platform your micro-app is running on, if you need to add platform specific features/content in your micro-app.

	- **DronaHQ.onIos:** returns *true* if the micro-app is running inside DronaHQ container on iOS.
	- **DronaHQ.onAndroid:** returns *true* if micro-app is running inside DronaHQ container on Android.
	- **DronaHQ.onWindowsPhone:** returns *true* if micro-app is running inside DronaHQ container on Windows Phone.
	- **DronaHQ.onWeb:** returns *true* if micro-app is running inside DronaHQ Web App on an HTML5 compliant browser.

Note that the DronaHQ Client must be fully initialized before making any Device API calls.

.. code:: javascript

	document.addEventListener('deviceready', function(){
		//Intialize your app
	}, false);

With this in mind, lets start by making a simple application that displays the following message on the screen.

.. image:: images/sample-qs.png
   :height: 100px
   :width: 500 px
   :scale: 100 %
   :align: center
   
Steps to Build

	- Step 1: Create the app project folder "DronaHQ QS". This will be the application's root directory.
	- Step 2: Include dronahq.js to the application's root directory.
	- Step 3: Add index.html, the web page that hosts the application, to the application's root directory.

	.. code:: html
	
		<html xmlns="http://www.w3.org/1999/xhtml">
			<head>
			<title>My First DronaHQ App</title>
			<meta name="viewport" content="width=device-width, initial-scale=1"/>
			<style>
				#dvAppMsg {
					width: 50%;
					margin: 50px auto;
					text-align: center;
					font-weight: 900;
					font-size: 30px;
					background-color: #ECECEC;
					padding: 5px;
					box-shadow: 0 2px 2px rgba(0,0,0,0.24),0 0 2px rgba(0,0,0,0.12);
				}
				@media (max-width:415px) {
					#dvAppMsg {
						width: 95%;
						font-size: 25px;
					}
				}
			</style>
			</head>
			<body>
				<div id="dvAppMsg"></div>
        
				<!-- Load dronahq.js -->
				<script src="dronahq.js"></script>
        
				<!-- Load index.js -->
				<!-- Write your application code in index.js -->
				<script src="index.js"></script>
			</body>
		</html>

	- Step 4: Add index.js to the application's root directory.

	.. code:: javascript

		(function () {
			// When DronaHQ Client App is initialized, 
			// an event 'deviceready' is triggered.
			// Add event listener for 'deviceready'.
			document.addEventListener('deviceready', function () {
        
				// Your application code when device is ready
                
				var msgEle = document.getElementById('dvAppMsg');
				msgEle.innerHTML = msgEle.innerHTML + 'My First DronaHQ App';
                
			}, false);
		})();

	- Step 5: Include all files in the application's root directory to a .ZIP file.
	- Step 6: :ref:`Deploy your application <micro-app-deployment>` as a .ZIP package named "**MyQS**".

Now open the client app, and on the homescreen a  micro-app icon named "**MyQS**" would be available. Click the micro-app to view your application.

You can also get started by trying out more of our |sample applications|.

.. |sample applications| raw:: html

   <a href="https://github.com/dronahq/samples" target="_blank">sample applications</a>





