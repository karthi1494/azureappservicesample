1. Open **WebApiConfig.cs** in **App_Start**
2. Add the following using statement after line 8

	```
	using System.Web.Http.Cors;
	```

3. Add the following code after line 23.

	<pre>
    // Enable CORS  
	config.EnableCors(new EnableCorsAttribute("*", "*", "*"));</pre>