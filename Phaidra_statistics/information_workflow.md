# Information Workflow

Once the user is authenticated (login), the client (Browser) sends data requests to the server using java services stored also in the server.

These java services return data that is used to populate the appropriate Widgets (charts, lists, grids, etc.) of the appropriate page in the client browser.


![](dataFlow.png)

##Data Workflow in detail: From Client to Server and back
In the application, "Service Variables" are used to call the java services and query the database. The Java Services return data to the Service Variables which will be used in turn by Local Variables in the client to feed the different Widgets.







![](dataflow-detail.png)

