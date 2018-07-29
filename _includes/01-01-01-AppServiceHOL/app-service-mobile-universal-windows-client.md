Once you have created your mobile app backend, you can follow an easy quickstart in the Azure Portal to either create a new app or modify an existing app to connect to your mobile app backend.

In this section you will create a new universal Windows app that is connected to your mobile app backend.

1. In the Azure Portal, click **Mobile App**, and then click the mobile app that you just created.

2. At the top of the blade, click **Add Client** and expand **Windows (C#)**.

   ![Mobile App quickstart steps](../images/01-01-01-AppServiceHOL/windows-quickstart.png)

   This displays the three easy steps to create a Windows Store app connected to your mobile app backend.

3. If you haven't already done so, download and install <a href="https://go.microsoft.com/fwLink/p/?LinkID=257546" target="_blank">Visual Studio Professional 2013</a> on your local computer or virtual machine.

4. Under **Download and run your app and service locally**, select a language for your Windows Store app, then click **Download**.

This downloads a solution contains projects for both the mobile app backend and for the sample _To do list_ application that is connected to your mobile app backend. Save the compressed project file to your local computer, upzip the compressed file and open Visual studio to edit the project. 

  ![](../images/01-01-01-AppServiceHOL/mobile-client-vs-projectview.png)

5. The mobile client application for Windows includes three projects. One projected *shared* by the two other Windows Phone and Windows 8.1 projects. the *shared* project includes, among other, the DataModel folder. Identify the **TodoItem.cs** file and add two properties as indicated by the following code snippet:

	<pre>
    public class TodoItem
	{
	    public string Id { get; set; }
	    
	    [JsonProperty(PropertyName = "text")]
	    public string Text { get; set; }
	    
	    [JsonProperty(PropertyName = "complete")]
	    public bool Complete { get; set; }
	    
	    [JsonProperty(PropertyName ="phonenumber")]
	    public string PhoneNumber { get; set; }
    }</pre>

6. Next you need to edit **MainPage.xaml** in the Windows 8.1 project. You need to add a new textbox for the phone number and a corresponding label. Locate the TextInput TextBox and then make the edits to match the following code snippet 

	<pre>
    &lt;StackPanel Orientation="Vertical" Margin="72,0,0,0"&gt;
    	&lt;TextBlock&gt;free text goes here&lt;/TextBlock&gt;
    	&lt;TextBox Name="TextInput" Margin="5" MinWidth="300"&gt;&lt;/TextBox&gt;
    	&lt;TextBlock&gt;phone number goes here</TextBlock&gt;
    	&lt;TextBox Name="PhoneInput" Margin="5" MinWidth="300"&gt;&lt;/TextBox&gt;	
    	&lt;Button Name="ButtonSave" Click="ButtonSave_Click" IsEnabled="False"&gt;Save&lt;/Button&gt;
    &lt;/StackPanel&gt;</pre>

7. At this stage your Windows 8.1 application should be ready to run and successfully connect to your mobile backend

 ![](../images/01-01-01-AppServiceHOL/mobile-client-windows8-1-running.png)
