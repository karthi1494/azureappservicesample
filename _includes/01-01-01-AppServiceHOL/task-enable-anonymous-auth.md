1. Open **ToDoItemController.cs**
2. Add the following using statement after line 9

	```
	using Microsoft.Azure.Mobile.Security;
	```

3. Paste the following Attribute above the class declaration
	
	```
	[AuthorizeLevel(AuthorizationLevel.Anonymous)]
	```