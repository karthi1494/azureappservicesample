Open Visual Studio 2013 and select **File > New Project**. Select the **ASP.NET Web Application** Template.  name the project *appserviceholapi*, and then click **OK**.

![](../images/01-01-01-AppServiceHOL/01-filenew-v3.png)

In the **New ASP.NET Project** dialog, select the **Empty** project template.

Click the **Web API** check box.

Clear the **Host in the cloud** check box.

Click **OK**.

![](../images/01-01-01-AppServiceHOL/webapinewproj.png)

Visual Studio creates project files for an empty Web API project.

![](../images/01-01-01-AppServiceHOL/sewebapi.png)

In **Solution Explorer**, right-click the project (not the solution), and then click **Add > Azure API App SDK**.

![](../images/01-01-01-AppServiceHOL/addapiappsdk.png)

In the **Choose API App Metadata source** dialog, click **Automatic Metadata Generation**, and then click **OK**.

![](../images/01-01-01-AppServiceHOL/chooseswagger.png)

This choice enables the dynamic Swagger UI, which you'll see later in the tutorial. For information about the static Swagger metadata file option, see [Convert an existing API to an API app](http://azure.microsoft.com/documentation/articles/app-service-dotnet-create-api-app-visual-studio). 

When you click **OK**, Visual Studio installs API app NuGet packages and adds API app metadata to the Web API project.  In the next section you see what was added.

### Review API app metadata

The metadata that enables a Web API project to be deployed as an API app is contained in an *apiapp.json* file and a *Metadata* folder.

![](../images/01-01-01-AppServiceHOL/metadatainse.png)

The default contents of the *apiapp.json* file resemble the following example:

		{
		    "$schema": "http://json-schema.org/schemas/2014-11-01/apiapp.json#",
		    "id": "appserviceholapi",
		    "namespace": "microsoft.com",
		    "gateway": "2015-01-14",
		    "version": "1.0.0",
		    "title": "appserviceholapi",
		    "summary": "",
		    "author": "",
		    "endpoints": {
		        "apiDefinition": "/swagger/docs/v1",
		        "status": null
		    }
		}

The *Metadata* folder contains information such as screenshots for the API App gallery or a static Swagger API definition file.

For this tutorial you don't need to modify any of this metadata. For more information about the format of *apiapp.json* and the contents of the *Metadata* folder, see [Convert an existing API to an API app](http://azure.microsoft.com/documentation/articles/app-service-dotnet-create-api-app-visual-studio). 

### Add Web API code

In the following steps you add code for a simple HTTP Get method that returns a hard-coded list of contacts. 

Right-click the **Models** folder in the Web API project, and then in the context menu select **Add > Class**. 

![](../images/01-01-01-AppServiceHOL/03-add-new-class-v3.png) 

Name the new file *todoitem.cs*, and then click **Add**. 

![](../images/01-01-01-AppServiceHOL/0301-add-new-class-dialog-v3.png) 

Replace the entire contents of the file with the following code. 

	namespace appserviceholapi.Models
	{
	    public class TodoItem
	    {
	        public string Id { get; set; }
	        public string Text { get; set; }
	        public bool Complete { get; set; }
	        public string PhoneNumber { get; set; }
	        public bool Processed { get; set; }
	    }
	}

Right-click the **Controllers** folder, and then in the context menu select **Add > Controller**. 

![](../images/01-01-01-AppServiceHOL/05-new-controller-v3.png)

In the **Add Scaffold** dialog, select the **Web API 2 Controller - Empty** option and click **Add**. 

![](../images/01-01-01-AppServiceHOL/06-new-controller-dialog-v3.png)

Name the controller **ValuesController** and click **Add**. 

![](../images/01-01-01-AppServiceHOL/07-new-controller-name-v2.png)

Replace the code in this file with the code below. 

	using appserviceholapi.Models;
	using System;
	using System.Collections.Generic;
	using System.Configuration;
	using System.Data;
	using System.Data.Common;
	using System.Data.SqlClient;
	using System.Web.Http;
	
	namespace appserviceholapi.Controllers
	{
	    public class ValuesController : ApiController
	    {
	        protected readonly string _connectionString;
	
	        public ValuesController()
	        {
	            _connectionString = ConfigurationManager.ConnectionStrings["DefaultConnection"].ConnectionString;
	        }
	
	        [HttpGet]
	        // GET api/values/
	        public IEnumerable<TodoItem> GetUnProcessedItems()
	        {
	            List<TodoItem> values;
	
	            using (SqlConnection conn = new SqlConnection(_connectionString))
	            {
	                using (SqlCommand selectCommand = new SqlCommand("SELECT [Id] ,[Text],[Complete],[PhoneNumber],[Processed] FROM [mobileHOL].[TodoItems] WHERE [Processed]=0 AND Complete=1", conn))
	                {
	                    conn.Open();
	                    values = new List<TodoItem>();
	                    foreach (DbDataRecord item in selectCommand.ExecuteReader())
	                    {
	                        values.Add(
	                            new TodoItem
	                            {
	                                Id = item["Id"].ToString(),
	                                Text = item["Text"].ToString(),
	                                PhoneNumber = item["PhoneNumber"].ToString(),
	                                Complete = Convert.ToBoolean(item["Complete"]),
	                                Processed = Convert.ToBoolean(item["Processed"])
	                            }
	                        );
	                    }
	                }
	            }
	
	            return values;
	        }
	
	        [HttpPatch]
	        // PATCH api/values/{id}
	        public void PatchUnProcessedItem(string id)
	        {
	            using(SqlConnection conn = new SqlConnection(_connectionString))
	            {
	                using (SqlCommand updateCommand = new SqlCommand("UPDATE [mobileHOL].[TodoItems] SET [Processed]=1 WHERE [Id]=@id", conn))
	                {
	                    conn.Open();
	
	                    // Assign user provided value to @id
	                    updateCommand.Parameters.Add("@id", SqlDbType.NVarChar, 128);
	                    updateCommand.Parameters["@id"].Value = id;
	
	                    updateCommand.ExecuteNonQuery();
	                }
	            }
	        }
	    }
	}

### Enable Swagger UI

By default, API App projects are enabled with automatic [Swagger](http://swagger.io/ "Official Swagger information") metadata generation, and if you used the **Add API App SDK** menu entry to convert a Web API project, an API test page is also enabled by default.  

However, the Azure API App new-project template disables the API test page. If you created your API app project by using the API App project template, you need to do the following steps to enable the test page.

Open the *App_Start/SwaggerConfig.cs* file, and search for **EnableSwaggerUI**:

![](../images/01-01-01-AppServiceHOL/12-enable-swagger-ui-with-box.png)

Uncomment the following lines of code:

        })
    .EnableSwaggerUi(c =>
        {

When you're done, the file should look like this:

![](../images/01-01-01-AppServiceHOL/13-enable-swagger-ui-with-box.png)

### Test the Web API

To view the API test page, run the app locally (CTRL-F5) and navigate to `/swagger`. 

![](../images/01-01-01-AppServiceHOL/14-swagger-ui.png)

Click the **Try it out** button, and you see that the API is functioning and returns the expected result. 

![](../images/01-01-01-AppServiceHOL/15-swagger-ui-post-test.png)