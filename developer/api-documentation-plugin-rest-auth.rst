.. _ref-rest-api-auth:

Authentication
==============
DronaHQ authenticates its REST API and restricts its access to various resources based on the scope of the API **tokenkey**. Any **tokenkey** provided by DronaHQ has a scope of  either:

	- Account
	- Micro-app

Account Scope
-------------
A **tokenkey** for the REST API that is scoped to an account provides access to all the resources for that account. You can generate a **tokenkey** with the 'account' scope from the |Settings| page in the DronaHQ Admin Console. This is very useful for testing the APIs, automating any tasks, or integrating your internally developed applications with the DronaHQ platform.

.. |Settings| raw:: html

   <a href="https://build.dronamobile.com/settings" target="_blank">Settings</a>

Please note that whenever you generate a new 'account' scoped **tokenkey**, all previous instances of 'account' scoped tokens are automatically invalidated.

Template Scope
--------------
A **tokenkey** with 'micro-app' scope is generated while registering a new micro-app in the DronaHQ admin console. All **tokenkeys** scoped to micro-apps allows access to resources constrained to the template where the micro-app resides. For examples: if an app is created in "Sales - France" template, it can only access users, groups, etc. mapped to that template. 

The 'micro-app' scoped keys are essential and useful for fine-grained control over the micro-app level authorization. Please note that a 'micro-app' scoped **tokenkey** cannot be regenerated. So if you have to regenerate the **tokenkey** with a micro-app scope, you will need to re-deploy/re-create the app.

