### Enable Anonymous Authentication in Mobile App

{% include 01-01-01-AppServiceHOL/task-enable-anonymous-auth.md %}

### Enable CORS in Mobile App

{% include 01-01-01-AppServiceHOL/task-enable-cors.md %}

### Create Angular Web Client

1. In Visual Studio, right-click on the Solution, Click **Add > New Project...** 
2. Select **ASP.NET Web Application**
3. Name the Project `webHOL`
4. In the New ASP.NET Project Dialogue select the **Empty** template
5. Create a new folder to the project called **App**
6. Create a new file in the App folder called **index.html**

	<pre>
	&lt;!DOCTYPE html&gt;
	&lt;html lang=&quot;en&quot;&gt;
	&lt;head&gt;
	    &lt;title&gt;MultiChannel ToDo&lt;/title&gt;
	    &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1&quot;&gt;
	    &lt;link href=&quot;lib/bootstrap/css/bootstrap.min.css&quot; rel=&quot;stylesheet&quot; /&gt;
	    &lt;link href=&quot;lib/bootstrap/css/bootstrap-theme.min.css&quot; rel=&quot;stylesheet&quot; /&gt;
	    &lt;link href=&quot;css/site.css&quot; rel=&quot;stylesheet&quot; /&gt;
	&lt;/head&gt;
	
	&lt;body ng-app=&quot;multiChannelToDo&quot;&gt;
	
	    &lt;div class=&quot;container&quot; ng-controller=&quot;ToDoController&quot;&gt;
	        &lt;h1&gt;&lt;span class=&quot;glyphicon glyphicon-list-alt&quot;&gt;&lt;/span&gt; To Do List&lt;/h1&gt;
	        &lt;div class=&quot;check-list-container&quot;&gt;
	            &lt;div class=&quot;check-list-item-container&quot;&gt;
	                &lt;ul class=&quot;check-list&quot;&gt;
	                    &lt;li id=&quot;check-list-item-{{item.id}}&quot; ng-repeat=&quot;item in items&quot; ng-hide=&quot;{{item.complete}}&quot; ng-click=&quot;complete(item)&quot; class=&quot;check-list-item&quot;&gt;&lt;span class=&quot;glyphicon glyphicon-check&quot; aria-hidden=&quot;true&quot;&gt;&lt;/span&gt; {{item.text}} - {{item.phoneNumber}}&lt;/li&gt;
	                &lt;/ul&gt;
	            &lt;/div&gt;
	        &lt;/div&gt;
	        &lt;hr class=&quot;&quot;/&gt;
	        &lt;div class=&quot;controls-container&quot;&gt;
	            &lt;form ng-submit=&quot;add()&quot; class=&quot;form-inline&quot;&gt;
	                &lt;div class=&quot;form-group&quot;&gt;
	                    &lt;input id=&quot;Text&quot; ng-model=&quot;itemText&quot; type=&quot;text&quot; class=&quot;form-control&quot; placeholder=&quot;Task&quot; /&gt;
	                    &lt;input id=&quot;PhoneNumber&quot; ng-model=&quot;itemPhone&quot; type=&quot;tel&quot; class=&quot;form-control&quot; placeholder=&quot;Phone Number&quot; /&gt;
	                    &lt;button class=&quot;btn btn-primary&quot;&gt;Add Item&lt;/button&gt;
	                &lt;/div&gt;
	            &lt;/form&gt;
	        &lt;/div&gt;
	    &lt;/div&gt;
	
	    &lt;script src=&quot;lib/jquery/jquery.min.js&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;
	    &lt;script src=&quot;lib/angular/angular.min.js&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;
	    &lt;script src=&quot;lib/bootstrap/js/bootstrap.min.js&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;
	    &lt;!-- AngularJS App --&gt;
	    &lt;script src=&quot;js/app.js&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;
	    &lt;script src=&quot;js/controllers/ToDoController.js&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;
	    &lt;script src=&quot;js/services/ToDoService.js&quot; type=&quot;text/javascript&quot;&gt;&lt;/script&gt;
	
	&lt;/body&gt;
	&lt;/html&gt;</pre>

7. create a new file in the App folder called **web.config**
	
	<pre>
	&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;
	&lt;!--
	  For more information on how to configure your ASP.NET application, please visit
	  http://go.microsoft.com/fwlink/?LinkId=169433
	  --&gt;
	&lt;configuration&gt;
	  &lt;system.web&gt;
	    &lt;compilation debug=&quot;true&quot; targetFramework=&quot;4.5&quot; /&gt;
	    &lt;httpRuntime targetFramework=&quot;4.5&quot; /&gt;
	  &lt;/system.web&gt;
	  &lt;appSettings&gt;
	    &lt;add key=&quot;apiPath&quot; value=&quot;&quot;/&gt;
	  &lt;/appSettings&gt;
	  &lt;system.webServer&gt;
	    &lt;staticContent&gt;
	      &lt;mimeMap fileExtension=&quot;eot&quot; mimeType=&quot;application/vnd.ms-fontobject&quot; /&gt;
	      &lt;mimeMap fileExtension=&quot;ttf&quot; mimeType=&quot;application/octet-stream&quot; /&gt;
	      &lt;mimeMap fileExtension=&quot;woff&quot; mimeType=&quot;font/x-woff&quot; /&gt;
	      &lt;mimeMap fileExtension=&quot;woff2&quot; mimeType=&quot;font/x-woff2&quot; /&gt;
	      &lt;mimeMap fileExtension=&quot;json&quot; mimeType=&quot;application/json&quot;/&gt;
	    &lt;/staticContent&gt;
	  &lt;/system.webServer&gt;
	&lt;/configuration&gt;</pre>

6. Create three (3) folders in App called **css**, **js**, and **lib**
7. Create a new file in the css folder called **site.css** and paste the following:

	<pre>
	.container {
	    width: 300px;
	    margin: auto;
	    margin-top: 60px;
	    padding: 10px;
	}
	
	ul.check-list {
	    -webkit-padding-start: 0px;
	}
	
	div.check-list-container {
	    width: 300px;
	    min-height: 200px;
	    text-align: right;
	}
	
	li.check-list-item {
	    height: 40px;
	    width: 225px;
	    padding-bottom: 2px;
	    list-style-type: none;
	    text-align: left;
	}
	
	    li.check-list-item span.glyphicon {
	        padding: 10px;
	    }
	
	div.controls-container {
	    text-align: right;
	}</pre>
	
8. Create a new file in the js folder called **app.js**

	<pre>
	var multiChannelToDoApp = angular.module('multiChannelToDo', []);
	var apiPath = "http://mobilehol-code.azurewebsites.net/tables";</pre>

9. Create two (2) folders in the js folder called **controllers** and **services**
10. Create a new file in the controllers folder called **ToDoController.js**

	<pre>
	'use strict';
	multiChannelToDoApp
	    .controller('ToDoController', ['$scope', 'toDoService', function ($scope, toDoService) {
	
	        $scope.get = function () {
	            toDoService.getItems()
	                .success(function (data) {
	                    $scope.items = data;
	            });
	        };
	
	        $scope.add = function () {
	            toDoService.add($scope.itemText, $scope.itemPhone)
	            .success(function(data){
	                $scope.itemText = '';
	                $scope.itemPhone = '';
	                $scope.get();
	            });
	        };
	
	        $scope.complete = function (item) {
	            toDoService.complete(item)
	                .success(function (data) {
	                    $scope.get();
	                });
	        };
	
	        $scope.get();
	
	}]);</pre>

11. Create a new file in the services folder called **ToDoService.js**

	<pre>
	'use strict';
	multiChannelToDoApp
	    .factory('toDoService', ['$http', function ($http) {
	        return {
	
	            getItems: function () {
	                return $http.get(apiPath + '/TodoItem');
	            },
	
	            add: function (task, phoneNumber) {
	                return $http.post(apiPath + '/TodoItem', { "text": task, "phoneNumber": phoneNumber, "complete": false });
	            },
	
	            complete: function (item) {
	                return $http.patch(apiPath + '/TodoItem/' + item.id, { "id": item.id, "text": item.text, "complete": true });
	            }
	        }
	    }]);</pre>

12. Create three (3) folders in the lib folder called **angular**, **bootstrap**, **jquery**
13. Download the minified Angular library from [code.angularjs.org](https://code.angularjs.org/1.3.6/angular.min.js)
14. Download the bootstrap library from [getbootstrap.com](https://github.com/twbs/bootstrap/releases/download/v3.3.2/bootstrap-3.3.2-dist.zip)
15. Download the minified jquery library from [jquery.com](http://code.jquery.com/jquery-2.1.3.min.js)

### Publish Angular Web Client

6.  In Solution Explorer, right-click the project and click **Publish**.

	![][DeployClickPublish]

7.	In Publish Web, click **Microsoft Azure Web Apps**.

	![][DeployClickWebSites]

8.	Click **Sign in**.

	![][DeploySignIn]

9.	Follow the prompts to log into your Azure account.

11. The Select Existing Web App dialog should now show you as signed in. Click **New**.

	![][DeployNewWebsite]  

12. In the **Web App name** field, specify a unique app name prefix. Your fully-qualified web app name will be *&lt;prefix>*.azurewebsites.net. Also, configure the **App Service plan**, **Resource group**, and **Region** fields. Then, click **Create**.

	![][DeploySiteSettings]

13.	The Publish Web dialog will be filled with the settings for your new web app. Click **Publish**.

	![][DeployPublishSite]

	Once Visual Studio finishes publishing the starter project to the Azure web app, the desktop browser opens to display the live web app.

     > Publishing the application will result in the app folder being deployed to the wwwroot directory. You will be met with a message which says "You do not have permission to view this directory or page." when the page loads from the browser launched by Visual Studio.

1. In Web App Blade for the Angular web app, click **Settings**
  
   ![](./images/01-01-01-AppServiceHOL/webhol-settings.png)

1. Click on **Application Settings**

   ![](./images/01-01-01-AppServiceHOL/webhol-appsettings.png)

1. Scroll to the Virtual Applications and Directory section at the bottom of the blade

   ![](./images/01-01-01-AppServiceHOL/webhol-appsettings-virtdir.png)

1. Add **app** to the end of the root virtual application
 
   ![](./images/01-01-01-AppServiceHOL/webhol-update-virtdir.png)

1. Click **Save**

   ![](./images/01-01-01-AppServiceHOL/webhol-virtdir-save.png)


### Validation Steps

1. Browse the Angular Web App or Refresh the browser window opened by Visual Studio.
1. Add a **new task** by filling in the following UI and hitting **Add Item**

   ![](./images/01-01-01-AppServiceHOL/webhol-addtask.png)

1. Verify the task is added to the list

   ![](./images/01-01-01-AppServiceHOL/webhol-mynewtask.png)

1. Click on the task and Verify it is cleared from the list

   ![](./images/01-01-01-AppServiceHOL/webhol-clearlist.png)


[DeployClickPublish]: ./images/01-01-01-AppServiceHOL/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-1.png
[DeployClickWebSites]: ./images/01-01-01-AppServiceHOL/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-2.png
[DeploySignIn]: ./images/01-01-01-AppServiceHOL/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-3.png
[DeployUsername]: ./images/01-01-01-AppServiceHOL/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-4.png
[DeployPassword]: ./images/01-01-01-AppServiceHOL/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-5.png
[DeployNewWebsite]: ./images/01-01-01-AppServiceHOL/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-6.png
[DeploySiteSettings]: ./images/01-01-01-AppServiceHOL/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7.png
[DeployPublishSite]: ./images/01-01-01-AppServiceHOL/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-8.png