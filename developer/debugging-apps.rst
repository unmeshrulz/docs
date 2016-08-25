Debugging Apps
==============

DronaHQ provides a specialized instance of DronaHQ Client that enables remote debugging for developers on Android and iOS. DronaHQ Client takes much of the pain out of debugging HTML5 applications across platforms.

We use a custom Chromium-based webview for DronaHQ Client on Android. Any application deployed in DronaHQ on Android 4.0 and above will have the same high-performance, feature-complete web runtime. In addition, developers will have a single version of Chrome to target and be able to debug on any Android 4.x device instead of the highly fragmented environment Android provides today.


Android Debugging
-----------------

Google has made developing web or hybrid applications on Android difficult for developers. Each version of Android has included a Webkit-based webview that lacked many advanced HTML5 features and had significant performance issues. Using the standard webview, developing a web application for Android involves testing, debugging, and working around Android's many platform incompatibilities and performance limitations. In addition, prior to Android 4.4, the webview did not support remote debugging, making this process particularly challenging.

DronaHQ Client simplifies web application debugging by providing a webview that supports remote debugging on all versions of Android 4.

Because DronaHQ is a security-focused product, we disable debugging for the production DronaHQ Client you can download from the Play Store. Instead, we provide a developer version of the DronaHQ Client for Android that developers can install directly on their Android devices.  Fill out this form to request for debuggable client app: |http://goo.gl/forms/6JDLA7N9x6|

.. |http://goo.gl/forms/6JDLA7N9x6| raw:: html
	
	<a href="http://goo.gl/forms/6JDLA7N9x6" target="_blank">http://goo.gl/forms/6JDLA7N9x6</a>
	
The debug client can be installed on the same device as the version from the Play Store. All data from each version of the app is isolated so it won't impact production data.

Next, if you haven't done any debugging on this device before, follow the instructions to enable remote debugging with Chrome: |https://developer.chrome.com/devtools/docs/remote-debugging|

.. |https://developer.chrome.com/devtools/docs/remote-debugging| raw:: html
	
	<a href="https://developer.chrome.com/devtools/docs/remote-debugging" target="_blank">https://developer.chrome.com/devtools/docs/remote-debugging</a>

Now that you have enabled remote debugging on your device, connect your device to your computer with a USB cable. Open Google Chrome on your computer and navigate to chrome://inspect
