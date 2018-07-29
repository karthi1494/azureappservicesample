Now, you need to create a new Logic app:

1. Click on the **+ New** button at the bottom-left of the screen, expand **Web + Mobile**, then click **Logic App**. 

   This displays the Create logic app blade, where you provide some basic settings to get started.

   ![Create logic app blade](../images/01-01-01-AppServiceHOL/createlogicapp.png)

2. In **Name** type a meaningful name for your logic app.

3. Choose the **App service plan** you used when creating your API App. This should automatically choose the Location, Subscription and Resource Group for you. This takes care of the basic settings, click **Create** to provision your empty logic application.

![A New logic app blade](../images/01-01-01-AppServiceHOL/logicapp-blade1.png)

### Adding a trigger
In the Logic app you will first add a trigger to your workflow.

4. On the Logic App blade, click the **Triggers and actions** part to open the editing canvas
    
    ![A New logic app blade](../images/01-01-01-AppServiceHOL/logicapp-editflow1.png)
    
    On the right side of the editor, you will find the API Apps in this resource group, which by now should include your API App that you created in previous steps, Mobile API that was auto generated for you  (we will ignore it for now), and two default (code-less) API Apps provided by Microsoft HTTP and Recurrence.

5. Click on the Recurrence API to add a trigger to your application.

6. Click the pen icon to edit the Recurrence setting. Set the **Frequency** to *Minutes* and the **Interval** to *1*. This will trigger the workflow to start every minute.  *Note : your plan must be Standard or above to run the logic every minute.*
    
    ![Call Web API App](../images/01-01-01-AppServiceHOL/logicapp-editflow2.png)

### Add the actions
Next we will add the actual actions themselves (the outgoing calls).

1. Click on the API App you created in the right pane and choose the **values_GetUnprocessedItems** action, which is the API that returns all *completed* items that have not been *processed* yet.

    ![Call Mobile Backend](../images/01-01-01-AppServiceHOL/logicapp-editflow3.png)

2. Next, add an HTTP API connector from the right pane. Choose *PATCH* as the action.

3. In the `...` menu at the top of the card select **Repeat over a list**. In the **Repeat** box select `apihol.code body`.

    In case there are multiple items returned from the API App, this line will force the HTTP API connector to iterate over all items that are returned from from your Web API connector call.

4. In the URI box, type your mobile code backend URL and add the following suffix `/tables/TodoItem/@{repeatItem().id}`
    As an example, for the URI looks like this- `http://mobilehol-code.azurewebsites.net/tables/TodoItem/@{repeatItem().id}`

5. In the **Body** box type `{"processed":true}` (this marks the processed flag as true).

    In REST APIs, the PATCH verb can take partial parts of the full body object. This will end up calling the mobile backend code *updating* the item identified by `repeatItem().id`

    ![Call Mobile Backend](../images/01-01-01-AppServiceHOL/logicapp-editflow4.png)

11. Save the flow you created.

### Viewing the  history of your Logic app
Return to the Logic App Blade. At this point your Logic App is ready to run and will start running automatically. At the bottom of the Logic App blade, you can see the state of the **All runs**.

A green check mark indicates successful run. Click on specific *run* instance shows all the steps of the workflow:

![](../images/01-01-01-AppServiceHOL/logicapp-AllRuns-oneitem.png)

Click on the API App action to view a summary of the step. Click on the **OUTPUT LINK** to view the output returned from the API App call.  

![](../images/01-01-01-AppServiceHOL/logicapp-AllRuns-oneitem-output.png)

Note: if you are not seeing any action on your Logic App. Try creating a new to do item in your mobile or web applications. Then mark that item as complete. This will trigger the workflow.
