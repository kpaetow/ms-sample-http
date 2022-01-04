# ms-sample-http

## About this project
_ms-sample-http_ is a near-verbatim working instance of the sample application provided by Microsoft as at Jan 4, 2022. This sample application uses the HTTP Server API v1.0.

The objective was to confirm the sample application can be used as a point of reference by getting into a compilable and executable state.

I had to make a few small modifications to the source code provided by Microsoft, as follows:
+ VS 2022 complained about the type of some of the parameters provided to the calls to **SentHttpResponse()** function in **DoReceiveRequests()**, so I had to prefix these with a type cast to (PSTR)
+ I added an endpoint (GET /end) to allow the application to terminate gracefully as **DoReceiveRequests()**, as provided, implements an endless loop and the app cannot terminate and clean up properly

<br>

> ### **NOTE**
> Normally one would not create an endpoint to allow remotely stopping a production web server unless it were part of a management API... Such an endpoint should be implemented to require an authorization token with administrative privileges

<br>

I also had to configure the compiled app to leverage UAC (User Account Control) at startup (via Project properties > Linker > Manifest File > UAC Execution Level) to request _highestAvailable_ execution privilege as the call to **HttpAddUrl()** is a privileged operation. If the app cannot get elevated privileges, the call fails with return code 5, which is a Windows system error code for "access denied." This is the case, notably, when attempting to debug the application in VS 2022 with the latter running under normal user privileges.

<br>

> ### **IMPORTANT**
> I gather the recommendation from Microsoft is to use the HTTP Server API v2.0 as it allows for HTTP worker processes to run with process isolation, and that can (and should) operate under normal user privileges. Only the "controller" process (the one that sets up the request handling processes) must have elevated privileges: https://docs.microsoft.com/en-us/windows/win32/http/process-isolation

<br>
<br>
<br>

# Links
Microsoft's HTTP Server API Tutorial:  
https://docs.microsoft.com/en-us/windows/win32/http/http-api-start-page

Microsoft's HTTP Server API Sample Application (NOTE: Uses v1.0 of the API):  
https://docs.microsoft.com/en-us/windows/win32/http/http-server-sample-application

