# ![](images5/media/image4.png) {#section .TOC-Heading}

# Contents {#contents .TOC-Heading}

[Introduction [3](#introduction)](#introduction)

[Dataflow Gen2 [3](#dataflow-gen2)](#dataflow-gen2)

[Task 1: Configure scheduled refresh for Supplier Dataflow
[3](#task-1-configure-scheduled-refresh-for-supplier-dataflow)](#task-1-configure-scheduled-refresh-for-supplier-dataflow)

[Pipeline [8](#pipeline)](#pipeline)

[Task 2: Create Pipeline
[8](#task-2-create-pipeline)](#task-2-create-pipeline)

[Task 3: Build simple Pipeline
[11](#task-3-build-simple-pipeline)](#task-3-build-simple-pipeline)

[Task 4: Create new Pipeline
[13](#task-4-create-new-pipeline)](#task-4-create-new-pipeline)

[Task 5: Create Until Activity
[14](#task-5-create-until-activity)](#task-5-create-until-activity)

[Task 6: Create Variables
[15](#task-6-create-variables)](#task-6-create-variables)

[Task 7: Configure Until Activity
[16](#task-7-configure-until-activity)](#task-7-configure-until-activity)

[Task 8: Configure Dataflow Activity
[22](#task-8-configure-dataflow-activity)](#task-8-configure-dataflow-activity)

[Task 9: Configure 1^st^ Set variable Activity
[24](#task-9-configure-1st-set-variable-activity)](#task-9-configure-1st-set-variable-activity)

[Task 10: Configure 2^nd^ Set variable Activity
[26](#task-10-configure-2nd-set-variable-activity)](#task-10-configure-2nd-set-variable-activity)

[Task 11: Configure 3^rd^ Set variable Activity
[27](#task-11-configure-3rd-set-variable-activity)](#task-11-configure-3rd-set-variable-activity)

[Task 12: Configure Wait Activity
[29](#task-12-configure-wait-activity)](#task-12-configure-wait-activity)

[Task 13: Configure Schedule Refresh for Pipeline
[32](#task-13-configure-schedule-refresh-for-pipeline)](#task-13-configure-schedule-refresh-for-pipeline)

[References [34](#references)](#references)

# Introduction 

We have ingested data from different data sources into the Lakehouse. In
this lab, you will set up a refresh schedule for the data sources. Just
to recap the requirement:

- **Supplier Data:** Snowflake is updated at midnight / 12 AM every day.

- **Employee Data:** in SharePoint is updated at 9 AM every day.
  However, we have noticed that sometimes there is a 5 -- 15 minute
  delay. We need to create a refresh schedule to accommodate this.

- **Customer Data:** in Dataverse is always up to date. Previously we
  refreshed this four times a day, at midnight / 12 AM, 6 AM, noon / 12
  PM, and 6 PM. Now, the IT team has created a link to Dataverse to
  ingest this data to an Admin Lakehouse. They have also transformed
  this data. We do not need to set up refresh as we are linking to the
  Lakehouse provided by the IT team.

- **Sales Data:** in ADLS is updated at noon / 12 PM every day. We do
  not need to set up refresh for this since we have created a shortcut.
  As soon as data is updated in ADLS, it is available.

By the end of this lab, you will have learned:

- How to configure a scheduled refresh of Dataflow Gen2

- How to create a Pipeline

- How to configure a scheduled refresh of a Pipeline

# Dataflow Gen2

### Task 1: Configure scheduled refresh for Supplier Dataflow

Let's start by configuring a scheduled refresh of Supplier Dataflow.

1.  Let's navigate back to the Fabric workspace, **FAIAD\_\<username\>**
    by selecting the workspace in the left panel.

2.  To maximize the panel with the list of artifacts, select the double
    arrow on the top right of the panel.

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image6.png){width="5.421303587051619in"
height="2.9585892388451445in"}

3.  All the artifacts you have created are listed here. On the right of
    the screen, in the **Search box** enter **df**. This will filter the
    artifacts to Dataflows.

![A screenshot of a
computer](images5/media/image7.png){width="5.162534995625546in"
height="1.536075021872266in"}

4.  Hover over the **df_Supplier_Snowflake** row. Select the **ellipsis
    (...)**.

5.  Notice there is option to Delete, Open, and Refresh the Dataflow.
    Let's look at Refresh history. Select **Recent Runs**.

![A screenshot of a
computer](images5/media/image8.png){width="3.4054166666666665in"
height="2.7854571303587052in"}

**Note:** A window/panel will appear on the right side showing a list of
refreshes

6.  You will notice that there is a singular refresh that executed when
    we selected the **Save and run** option in the previous lab. The
    **Type** of refresh we can see is listed as **On Demand** which lets
    us know this was a manually executed refresh.

![A close-up of a computer screen AI-generated content may be
incorrect.](images5/media/image9.png){width="5.638132108486439in"
height="1.1517213473315835in"}

7.  Select the **Start time** link.

**Note:** Start time will be different for you.

![A screenshot of a
computer](images5/media/image10.png){width="5.324896106736658in"
height="1.5377340332458442in"}

Details screen will open. This will provide details of the refresh. It
lists the start time, end time, and duration. It also lists the tables /
activities that were refreshed. In case there is a failure, you can
click on the name of the table / activity to investigate further.

![A screenshot of a
computer](images5/media/image11.png){width="4.266701662292213in"
height="3.4329615048118987in"}

8.  Let's navigate away, by clicking on the **X** on the top right
    corner. You will be navigated back to the **workspace**.

9.  Hover over the **df_Supplier_Snowflake** row. Select the **ellipsis
    (...)**.

10. Let's see how we can schedule a refresh to happen automatically.
    Choose the **Settings** option.\
    \
    ![A screenshot of a
    computer](images5/media/image12.png){width="5.483063210848644in"
    height="1.9905391513560804in"}

11. You will see in the **Settings** panel that appeared we have three
    options: **\
    About --** Here we can change the name of the Dataflow and add a
    description. Also, we can see who is the owner of the dataflow and
    the last time it was modified.\
    **Endorsement --** This allows us to specify if the dataflow will
    carry the **Promoted** or the **Certified** tag for others to see.\
    **Schedule -** This is where we can schedule out dataflows.\
    ![A screenshot of a
    computer](images5/media/image13.png){width="5.259491469816273in"
    height="2.7353849518810147in"}

12. Select the **Schedule** option

13. To activate a schedule, we simply must click **Add Schedule**

![A screenshot of a schedule AI-generated content may be
incorrect.](images5/media/image14.png){width="3.797930883639545in"
height="2.308015091863517in"}

14. This now allows us to specify the cadence of the refresh by
    selecting an option for the **Repeat** property. For this scenario
    we can choose **Daily (1)**

15. For the **Time** property we can specify **12:00 AM** **(2)** since
    we want midnight

**Note:** By clicking on Add another time link, you can add multiple
refresh times.

16. We can also specify a **Start date and time (3)** as well as an
    **End date and time (4)**. For this scenario simply choose whatever
    the current day is for the start date and the end date.

17. You can specify which **Time Zone (5)** you would like the times to
    represent. Lastly select **Save**

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image15.png){width="4.970833333333333in"
height="3.9162412510936133in"}

18. You will see the scheduled refresh and be able to edit or delete it
    if it is no longer necessary or add other scheduled refreshes.

![](images5/media/image16.png){width="6.5in"
height="4.286111111111111in"}

As mentioned earlier, we need to build custom logic to handle the
scenario where the Employee file in SharePoint is not delivered on time.
Let's use a Pipeline to solve this.

# Pipeline

### Task 2: Create Pipeline

1.  Let's navigate back to the Fabric workspace, **FAIAD\_\<username\>**
    by selecting the workspace in the left panel.

2.  From the top menu select **+ New item (1) -\> Pipeline (2).** ![A
    screenshot of a chat AI-generated content may be
    incorrect.](images5/media/image17.png){width="6.501931321084864in"
    height="3.15048665791776in"}

3.  A new pipeline dialog opens. Name the pipeline as
    **pl_Refresh_People_SharePoint** and select **Create**.

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image18.png){width="3.016321084864392in"
height="2.349073709536308in"}

You are navigated to the **Pipeline page**. If you have worked with
Azure Data Factory, this screen will be familiar. Let's get a quick
overview of the layout.

You are on the **Home** screen. If you look at the top menu, you will
find options to add the commonly used activities: validate, run a
pipeline, and view the run history. Also, in the center pane, you will
find quick options to start building the pipeline.

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image19.png){width="6.112006780402449in"
height="3.7525699912510935in"}

4.  From the top menu select **Activities**. Now in the menu you will
    find a list of commonly used Activities.

5.  Select the **ellipsis (...)** on the right on the menu to view all
    the other available Activities. We are going to use a few of these
    Activities in the lab.

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image20.png){width="5.182053805774278in"
height="2.36374343832021in"}

6.  From the top menu click **Run**. You will find options to run and
    schedule the pipeline execution. You will also find the option to
    view execution history by using View run history.

7.  From the top menu select **View**. Here you will find options to
    view the code in JSON format. You will also find options to auto
    align the activities.

**Note:** If you have a JSON background, at the end of the lab, feel
free to select View JSON code. Here you will notice all the
orchestration you are doing using the design view can also be written in
JSON.

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image21.png){width="5.84456583552056in"
height="0.8855402449693788in"}

### Task 3: Build simple Pipeline

Let's start building the pipeline. We need an activity to refresh the
Dataflow. Let's find an activity which we can use.

1.  From the top menu select **Activities -\> Dataflow**. Dataflow
    activity is added to the center design pane. Notice the bottom pane
    now has configuration options of the Dataflow activity.

2.  We are going to configure the activity to connect to
    df_People_SharePoint dataflow. From the **bottom** **pane**, select
    **Settings**.

*Note: You may need to drag the bottom pane up to see settings.*

![](images5/media/image22.png){width="5.101750874890639in"
height="0.7440048118985126in"}

3.  Make sure **Workspace** is set to your Fabric workspace,
    **FAIAD\_\<username\>.**

4.  From the **Dataflow dropdown** select **df_People_SharePoint**. When
    this Dataflow activity is executed, it is going to refresh
    **df_People_SharePoint.** That was easy, right?

In our scenario, Employee Data is not updated on a schedule. Sometimes
there is a delay. Let's see if we can accommodate this.

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image23.png){width="6.5in"
height="3.370138888888889in"}

5.  From the **bottom** **pane**, select **General**. Let's give the
    activity a name and description.

6.  In the **Name** field, enter **dfactivity_People_SharePoint**

7.  In the **Description** field, enter **Dataflow activity to refresh
    df_People_Sharepoint dataflow.**

8.  Notice there is an option to Deactivate an activity. This feature is
    useful during testing or debugging. Leave it as **Activated**.

9.  There is an option to set **Timeout**. Let's leave the **default
    value** as is which should give enough time for the dataflow to
    refresh.

**Note:** Since the data is not available on a schedule, let's set the
activity to re-execute every 10 minutes, three times. If it fails on the
third attempt as well, then it will report a failure.

10. Set **Retry** to **3**

11. Expand **Advanced** section.

12. Set **Retry interval (sec)** to **600**.

13. From the menu select **Home -\> Save** icon to save the pipeline.

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image24.png){width="4.333333333333333in"
height="5.917297681539807in"}

Notice the advantage of using the pipeline compared to setting the
dataflow on scheduled refresh (like we did for the earlier dataflow):

- Pipeline provides the option to retry multiple times before failing
  the refresh.

- Pipeline provides the ability to perform other tasks as well as
  refreshing the dataflow

### Task 4: Create new Pipeline

Let's add a little more complexity to our scenario. We have noticed that
if the data is not available at 9 AM, then typically it is available
within five minutes. If the time window is missed, then it takes 15
minutes for the file to be available. We want to schedule the retries at
five and 15 minutes. Let's see how this can be achieved by creating a
new Pipeline.

1.  From the left panel, click **FAIAD\_\<username\>**, to be navigated
    to the workspace home.

2.  From the top menu, click **+ New item (1)** and from the popout
    window**,** click **Pipeline (2)**.

![A screenshot of a search engine AI-generated content may be
incorrect.](images5/media/image25.png){width="4.881148293963254in"
height="3.552610454943132in"}

3.  New pipeline dialog opens. **Name** the pipeline as
    **pl_Refresh_People_SharePoint_Option2 (3),** and select **Create
    (4)**.

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image26.png){width="3.510906605424322in"
height="2.792055993000875in"}

### Task 5: Create Until Activity

1.  You will be navigated to the Pipeline screen. From the menu, select
    **Activities**.

2.  Click the **ellipsis(...)** on the right.

3.  From the activity list, click **Until**.

**Until**: is an activity that is used to iterate until a condition is
satisfied.

In our scenario, we are going to iterate and refresh the dataflow until
it is successful, or we have tried three times.

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image27.png){width="6.5in"
height="3.9118055555555555in"}

### Task 6: Create Variables

1.  We need to create variables which will be used to iterate and set
    status. Select the **blank area** in the pipeline design pane.

2.  Notice the menu in the bottom pane changes. Select **Variables**.

3.  Select **+ New** to add a new variable.

4.  Notice a row appears. Enter **varCounter** in the **Name text box**.
    We will use this variable to iterate three times.

5.  From **Type** **dropdown** select **Integer**.

6.  Enter **Default value** of **0**.

**Note:** we are appending variable names with var, so it is easy to
find them, and it is good practice.

![A screenshot of a
computer](images5/media/image28.png){width="4.0380347769028875in"
height="3.37667760279965in"}

7.  Select **+** **New** to add another new variable.

8.  Notice a row appears. Enter **varTempCounter** in the **Name text
    box**. We are going to use this variable increment varCounter
    variable.

9.  From **Type** **dropdown** select **Integer**.

10. Enter **Default value** of **0**.

11. Follow similar steps to add three more variables:

    a.  **varIsSuccess** of type **String** and default value **No**.
        This variable will be used to indicate if the dataflow refresh
        was successful.

    b.  **varSuccess** of type **String** and default value **Yes**.
        This variable will be used to set the value of varIsSuccess if
        dataflow refresh is successful.

    c.  **varWaitTime** of type **Integer** and default value **60**.
        This variable will be used to set the wait time if dataflow
        fails. (Either 5 minutes/300 seconds or 15 minutes/900 seconds.)

**Note:** Make sure there is no space before or after the variable name.

![A screenshot of a
computer](images5/media/image29.png){width="5.335777559055118in"
height="5.0535968941382325in"}

### Task 7: Configure Until Activity

1.  Select **Until** activity.

2.  From the **bottom pane**, select **General**.

3.  Enter **Name** as **Iterator**

4.  Enter **Description** as "**Iterator to refresh dataflow. It will
    retry up to 3 times**".

![A screenshot of a
computer](images5/media/image30.png){width="3.4072014435695537in"
height="3.7805938320209975in"}

5.  From the bottom pane, select **Settings (1)**.

6.  Select the **Expression text box (2)**. We need to enter an
    expression in this text box that will evaluate to true or false. The
    Until activity will continue to iterate while this expression
    evaluates to false. Once the expression evaluates to true, the Until
    activity stops the iteration and moves on to the next activity.

7.  Select **Add dynamic content (3)** link that appears below the text
    box.

![A screenshot of a
computer](images5/media/image31.png){width="4.065559930008749in"
height="3.6489752843394574in"}

We need to write an expression which would execute until either the
value of **varCounter is 3** or value **varIsSuccess is Yes.**
(varCounter and varIsSuccess are the variables we just created.)

8.  **Pipeline expression builder** dialog opens. In the bottom half of
    the dialog, you will have a menu:

    a.  **Parameters:** Values that are passed to the pipeline. E.g.
        Value from one pipeline passed to another pipeline. These values
        can be used in any expression but cannot be changed during the
        pipeline run.

    b.  **System variables: C**an be used in expressions when defining
        entities within either service. E.g., pipeline id, pipeline
        name, trigger name, etc.

    c.  **Trigger parameters:** Parameters that triggered the pipeline.
        E.g., File Name or Folder Path.

    d.  **Functions:** You can call functions within expressions.
        Functions are categorized into Collection, Conversion, Date,
        Logical, Math, and String functions. E.g., concat is a String
        function, add is a Math function, etc.

    e.  **Variables:** Pipeline variables are values that can be set and
        modified during a pipeline run. Unlike pipeline parameters,
        which are defined at the pipeline level and cannot be changed
        during a pipeline run, pipeline variables can be set and
        modified within a pipeline using a Set Variable activity. We are
        going to use Set Variable activity shortly.

![A screenshot of a
computer](images5/media/image32.png){width="4.497398293963254in"
height="4.83374343832021in"}

9.  Click **Functions** from the menu.

10. From the **Logical Functions** section, select **or** function.
    Notice **\@or()** is added to the dynamic expression text box. The
    **or** function takes two parameters, we are working on the first
    parameter.

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image33.png){width="3.9608552055993003in"
height="5.232325021872266in"}

11. Place the cursor **in between the parentheses** of the **\@or**
    function.

12. From the **Logical Functions** section, select **equals** function.
    Notice this is added to the dynamic expression text box.

**Note:** Your function should look like **\@or(equals())**. The equals
function also takes two parameters. We will be checking if the variable
varCounter is equal to 3.

![A screenshot of a
computer](images5/media/image34.png){width="3.66491469816273in"
height="3.2867891513560803in"}

13. Now place the cursor **in between the parentheses** of **\@equals**
    function to add the parameters.

14. From the bottom menu, select **Variables**.

15. Select **varCounter** variable which will be the first parameter.

16. Enter **3** as the second parameter of the equals function. Like the
    screenshot below, your expression will be
    **\@or(equals(variables(\'varCounter\'),3))**

![A screenshot of a
computer](images5/media/image35.png){width="3.7630205599300086in"
height="3.908770778652668in"}

17. We need to add the second parameter to the **or** function. **Add a
    comma** in between the ending two parentheses. This time we will try
    typing in the function name. Start typing **equ** and you will get a
    drop down of available functions (this is called IntelliSense).
    Select the **equals** function.

![A screenshot of a
computer](images5/media/image36.png){width="3.211155949256343in"
height="3.2165988626421695in"}

18. The first parameter of equals function is a variable. Place **cursor
    before the comma**.

19. Start typing **variables(**

20. With the help of IntelliSense select **variables(\'varIsSuccess\')**

21. After the comma, let's enter the second parameter. Start typing
    **variables(**

22. With the help of IntelliSense select **variables(\'varSuccess\')**.
    Here we are comparing the value of varIsSuccess to the value of
    varSuccess. (varSuccess is defaulted to Yes.)

![A screenshot of a computer program AI-generated content may be
incorrect.](images5/media/image37.png){width="6.365471347331583in"
height="2.7399660979877516in"}

23. Your expression should be:

**\@or(equals(variables(\'varCounter\'),3),equals(variables(\'varIsSuccess\'),
variables(\'varSuccess\')))**

24. Select **OK**.

![A screenshot of a
computer](images5/media/image38.png){width="3.3075109361329833in"
height="3.6492672790901137in"}

### Task 8: Configure Dataflow Activity

1.  You will be navigated back to the design screen. With **Until
    activity** selected, from the **bottom pane**, select
    **Activities**. We will now add the activities that need to be
    executed.

2.  Select the **Edit icon** in the first row. You will be navigated to
    a blank iterator design screen.

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image39.png){width="3.84285542432196in"
height="3.466781496062992in"}

3.  From the top menu, select **Activities -\> Dataflow**. Dataflow
    activity is added to the design pane.

4.  With **Dataflow activity selected**, in the bottom pane select
    **General**. Let's give the activity a name and description.

5.  In the **Name** field, enter **dfactivity_People_SharePoint**

6.  In the **Description** field, enter "**Dataflow activity to refresh
    df_People_Sharepoint dataflow".**

![A screenshot of a
computer](images5/media/image40.png){width="2.861356080489939in"
height="3.9495548993875764in"}

7.  Select **Settings** from the bottom pane.

8.  Make sure **Workspace** is set to your workspace,
    **FAIAD\_\<username\>.**

9.  From the **Dataflow dropdown** select **df_People_SharePoint**.

![A screenshot of a
computer](images5/media/image41.png){width="3.7470592738407698in"
height="3.231913823272091in"}

### Task 9: Configure 1^st^ Set variable Activity

We have configured the Dataflow activity like we did earlier in the lab.
Now we will add new logic. If the dataflow refresh is successful, we
need to exit out of the Until iterator. Remember one of the conditions
to exit the iterator is to set the value of varIsSuccess variable to
Yes.

1.  From the top menu, select **Activities -\> Set variable**. Set
    variable activity is added to the design canvas.

2.  With **Set variable activity** selected, in the bottom pane select
    **General**. Let's give the activity a name and description.

3.  In the **Name** field, enter **set_varIsSuccess**

4.  In the **Description** field, enter "**Set variable varIsSuccess to
    Yes".**

**Note:** Hover over **Dataflow activity**. To the right of the activity
box there are four icons. These can be used to connect to the next
activity based on the result of the activity:

a.  **Grey curved arrow** icon is used on skip of the activity.

b.  **Green check mark** icon is used on success of the activity.

c.  **Red x-mark** icon is used on failure of the activity.

d.  **Blue straight arrow** icon is used on completion of the activity.

<!-- -->

5.  Click the **green check mark** from dfactivity_People_SharePoint
    Dataflow activity and drag to connect to the new
    **set_varIsSuccess** **Set variable activity**. So, on success of
    dataflow refresh we want to execute the Set variable activity.

![A screenshot of a
computer](images5/media/image42.png){width="5.4834044181977255in"
height="3.5407797462817148in"}

6.  With **Set variable activity** selected, click **Settings** from the
    bottom menu.

7.  In the bottom pane, make sure **Variable type** is **Pipeline
    variable**.

8.  In the **Name** field, select **varIsSucces.** This is the variable
    whose value we are going to set.

9.  In the **Value** field, select the **text box**. Select **Add
    dynamic content** link.

![A screenshot of a
computer](images5/media/image43.png){width="3.1692147856517936in"
height="3.428713910761155in"}

10. Pipeline expression builder dialog opens. Select the **Add dynamic
    content below using any combination of expressions, functions, and
    system variables** text area **(1)**.

11. From the bottom menu, click on the **elipses(\...) (2)** select
    **Variables (3) -\> varSuccess (4)**. Notice
    **\@variables('varSuccess')** is entered in the Add dynamic content
    below text area. Remember when we created variables, we had preset
    the value of varSuccess variable to Yes. So, we are assigning the
    value of Yes to the varIsSuccess variable.

12. Select **OK**. You will be navigated back to the **iterator design
    pane**.

![A screenshot of a
computer](images5/media/image44.png){width="3.854722222222222in"
height="3.6649300087489065in"}

Now we need to set the counter if the dataflow activity fails. In a
Pipeline, we cannot self-reference a variable. Which means we cannot
increment the counter variable varCounter by adding one to its value
(varCounter = varCounter + 1). So, we make use of the varTempCounter
variable.

### Task 10: Configure 2^nd^ Set variable Activity

1.  From the top menu, select **Activities -\> Set variable**. Set
    variable activity is added to the design canvas.

2.  With **Set variable activity** selected, in the bottom pane select
    **General**. Let's give the activity a name and description.

3.  In the **Name** field, enter **set_varTempCounter**

4.  In the **Description** field, enter "**Increment variable
    varTempCounter".**

5.  Click the **red x-mark** from Dataflow activity to the new Set
    variable activity. So, on failure of dataflow refresh we want to
    execute this Set variable activity.

![A screenshot of a
computer](images5/media/image45.png){width="5.41128937007874in"
height="3.526010498687664in"}

6.  With **Set variable activity** selected, select **Settings** from
    the bottom menu.

7.  In the bottom pane, make sure **Variable type** is **Pipeline
    variable**.

8.  In the **Name** field, select **varTempCounter.** This is the
    variable whose value we are going to set.

9.  In the **Value** field, select the **text box**. Select **Add
    dynamic content** link.

10. Pipeline expression builder dialog opens. Enter
    **\@add(variables(\'varCounter\'),1)**

**Note:** Feel free to type this expression in, use the menu to select
the functions, or copy and paste it. This function is setting the value
of variable varTempCounter to the value of variable varCounter plus one,
(varTempCounter = varCounter + 1).

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image46.png){width="5.578041338582677in"
height="3.4153576115485564in"}

Now we need to set the value of varCounter variable to the value of
varTempCounter.

### Task 11: Configure 3^rd^ Set variable Activity

1.  From the top menu, select **Activities -\> Set variable**. Set
    variable activity is added to the design canvas.

2.  With **Set variable activity** selected, in the bottom pane select
    **General**. Let's give the activity a name and description.

3.  In the **Name** field, enter **set_varCounter**

4.  In the **Description** field, enter "**Increment variable
    varCounter".**

5.  Click the **green check mark** from set_varTempCounter Set variable
    activity and drag to connect to the new **set_varCounter Set
    variable activity**.

![A screenshot of a
computer](images5/media/image47.png){width="5.820909886264217in"
height="3.7313527996500437in"}

6.  With **set_varCounter Set variable activity** selected, click
    **Settings** from the bottom menu.

7.  In the bottom pane, make sure **Variable type** is **Pipeline
    variable**.

8.  In the **Name** field, select **varCounter**. This is the variable
    whose value we are going to set.

9.  In the **Value** field, select the **text box**. Select **Add
    dynamic content** link.

10. Pipeline expression builder dialog opens. Enter
    **\@variables(\'varTempCounter\')**. Feel free to type this
    expression in, or use the menu to select the functions, or copy and
    paste it in.

11. Click on OK.

![](images5/media/image48.png){width="4.76306539807524in"
height="3.0593536745406826in"}

**Note:** This function sets the value of variable varCounter to the
value of variable varTempCounter (varCounter = varTempCounter). At the
end of each iteration both varCounter and varTempCounter have the same
value.

### Task 12: Configure Wait Activity

Next, we need to wait for 5 minutes/300 seconds if dataflow refresh
fails the first time before trying again. If the dataflow refresh fails
for the second time, we need to wait 15 minutes/900 seconds and try
again. We are going to use Wait activity and variable varWaitTime to set
the wait time.

1.  From the top menu, select **Activities -\> ellipsis (...) -\>
    Wait**. Wait activity is added to the design canvas.

2.  With the **Wait activity** selected, in the bottom pane select
    **General**. Let's give the activity a name and description.

3.  In the **Name** field, enter **wait_onFailure**

4.  In the **Description** field, enter "**Wait for 300 seconds on 2nd
    try and 900 seconds on 3rd try".**

5.  Click the **green check mark** from set_varCounter Set variable
    activity and drag to connect to the new **wait_onFailure Wait
    activity**.

![A screenshot of a
computer](images5/media/image49.png){width="5.0399343832021in"
height="2.5506594488188976in"}

6.  With **Wait activity** selected, click **Settings** from the bottom
    menu.

7.  In the **Wait time in seconds** field, select the **text box** and
    select **Add dynamic content** link.

8.  Pipeline expression builder dialog opens. Enter

> **\@if(**
>
> **greater(variables(\'varCounter\'), 1),**
>
> **if(equals(variables(\'varCounter\'), 2),**
>
> **mul(variables(\'varWaitTime\'),15 ),**
>
> **mul(variables(\'varWaitTime\'), 0)**
>
> **),**
>
> **mul(variables(\'varWaitTime\'),5 )**
>
> **)**

Feel free to type this expression in, or use the menu to select the
functions, or copy and paste it in.

![A screenshot of a
computer](images5/media/image50.png){width="5.4974857830271215in"
height="4.127225503062117in"}

We are using two new functions here:

- **greater:** Takes two numbers as parameters and compares which one is
  greater.

- **mul:** This is a multiply function, it takes in two parameters to
  multiply.

The expression is a nested if statement. It is checking if the value of
varCounter variable is greater than 1.

If it is true, it checks if the value of varCounter variable is 2. If it
is true, it set the wait time to varWaitTime times 15. Remember, we had
defaulted varWaitTime value to 60. That would be 60\*15 = 900 seconds.
If the value of varCounter variable is not 2 (it is greater than 2,
which means dataflow refresh has failed 3 times we are done iterating.
We don't have to wait anymore), wait time is set to varWaitTime \* 0.
So, to 0. If the value of varCounter variable is 1, then we multiply the
varWaitTime \* 5. That would be 60\*5 = 300 seconds.

9.  Select **OK**.

**Checkpoint:** Your **Until** iterator should look like the screenshot
below.

![A screenshot of activities in Until
activity](images5/media/image51.png){width="6.5in"
height="3.0236100174978127in"}

10. From the top left of the design canvas select
    **pl_Refresh_People_Sharepoint_Option2** or **Main Canvas** to be
    navigated out of Until iterator.

![A screenshot of a computer AI-generated content may be
incorrect.](images5/media/image52.png){width="6.5in"
height="2.3895833333333334in"}

11. We are done creating the pipeline. From the top menu, select **Home
    -\> Save icon** to save the pipeline.

![A screenshot of a
computer](images5/media/image53.png){width="3.246725721784777in"
height="3.098792650918635in"}

### Task 13: Configure Schedule Refresh for Pipeline

1.  We can test the pipeline, by selecting **Home -\> Run.\
    \
    Note:** It may take a few minutes for the pipeline to complete
    refresh. This is a training environment, so the file in SharePoint
    is always available. Hence, your pipeline will never fail.

2.  We can set the pipeline to execute on a schedule. From the top menu,
    select **Home -\> Schedule**. Schedule dialog opens.

3.  Select the **Add Schedule** button below **Scheduled run**.

![A screenshot of a schedule AI-generated content may be
incorrect.](images5/media/image54.png){width="4.118082895888014in"
height="2.417713254593176in"}

4.  Set **Repeat dropdown** to **Daily**.

5.  Set **Time** to **9 AM**.

6.  Set **Start date and time** to **Today**.

7.  Set **End date and time** to a **future date**.

8.  Set your **Time zone**.

**Note**: Since this is a lab environment, you can set the time zone to
your preferred time zone. In a real scenario, you will be setting the
time zone based on your / data source location.

9.  Select **Save**.

10. Select the **X** mark on the top right of the dialog to close it.

![A screenshot of a schedule AI-generated content may be
incorrect.](images5/media/image55.png){width="4.932127077865267in"
height="4.948604549431321in"}

11. Select your Fabric workspace **FAIAD\_\<username\>** in the left
    panel to navigate to the workspace**.**

**Note**: In the Schedule screen, there is no option to notify on
success or failure (like Dataflow Schedule). Notification can be done by
adding an activity in the pipeline. We are not doing it in this lab
because this is a lab environment.

We have scheduled refreshes for the various data sources. We will create
a semantic model with relationships, measures and other modeling
operations in the next lab.

# References

Fabric Analyst in a Day (FAIAD) introduces you to some of the key
functions available in Microsoft Fabric. In the menu of the service, the
Help (?) section has links to some great resources.

![A screenshot of help
options](images5/media/image56.png){width="1.8736504811898513in"
height="4.344214785651793in"}

Here are a few more resources that will help you with your next steps
with Microsoft Fabric.

- See blog post to read the full [Microsoft Fabric GA
  announcement](https://aka.ms/Fabric-Hero-Blog-Ignite23)

- Explore Fabric through the [Guided
  Tour](https://aka.ms/Fabric-GuidedTour)

- Sign up for the [Microsoft Fabric free
  trial](https://aka.ms/try-fabric)

- Visit the [Microsoft Fabric website](https://aka.ms/microsoft-fabric)

- Learn new skills by exploring the [Fabric Learning
  modules](https://aka.ms/learn-fabric)

- Explore the [Fabric technical
  documentation](https://aka.ms/fabric-docs)

- Read the [free e-book on getting started with
  Fabric](https://aka.ms/fabric-get-started-ebook)

- Join the [Fabric community](https://aka.ms/fabric-community) to post
  your questions, share your feedback, and learn from others

Read the more in-depth Fabric experience announcement blogs:

- [Data Factory experience in Fabric
  blog](https://aka.ms/Fabric-Data-Factory-Blog) 

- [Synapse Data Engineering experience in Fabric
  blog](https://aka.ms/Fabric-DE-Blog) 

- [Synapse Data Science experience in Fabric
  blog](https://aka.ms/Fabric-DS-Blog) 

- [Synapse Data Warehousing experience in Fabric
  blog](https://aka.ms/Fabric-DW-Blog) 

- [Synapse Real-Time Analytics experience in Fabric
  blog](https://aka.ms/Fabric-RTA-Blog)

- [Power BI announcement blog](https://aka.ms/Fabric-PBI-Blog)

- [Data Activator experience in Fabric
  blog](https://aka.ms/Fabric-DA-Blog) 

- [Administration and governance in Fabric
  blog](https://aka.ms/Fabric-Admin-Gov-Blog)

- [OneLake in Fabric blog](https://aka.ms/Fabric-OneLake-Blog)

- [Dataverse and Microsoft Fabric integration
  blog](https://aka.ms/Dataverse-Fabric-Blog)

> © 2023 Microsoft Corporation. All rights reserved.
>
> By using this demo/lab, you agree to the following terms:
>
> The technology/functionality described in this demo/lab is provided by
> Microsoft Corporation for purposes of obtaining your feedback and to
> provide you with a learning experience. You may only use the demo/lab
> to evaluate such technology features and functionality and provide
> feedback to Microsoft. You may not use it for any other purpose. You
> may not modify, copy, distribute, transmit, display, perform,
> reproduce, publish, license, create derivative works from, transfer,
> or sell this demo/lab or any portion thereof.
>
> COPYING OR REPRODUCTION OF THE DEMO/LAB (OR ANY PORTION OF IT) TO ANY
> OTHER SERVER OR LOCATION FOR FURTHER REPRODUCTION OR REDISTRIBUTION IS
> EXPRESSLY PROHIBITED.
>
> THIS DEMO/LAB PROVIDES CERTAIN SOFTWARE TECHNOLOGY/PRODUCT FEATURES
> AND FUNCTIONALITY, INCLUDING POTENTIAL NEW FEATURES AND CONCEPTS, IN A
> SIMULATED ENVIRONMENT WITHOUT COMPLEX SET-UP OR INSTALLATION FOR THE
> PURPOSE DESCRIBED ABOVE. THE TECHNOLOGY/CONCEPTS REPRESENTED IN THIS
> DEMO/LAB MAY NOT REPRESENT FULL FEATURE FUNCTIONALITY AND MAY NOT WORK
> THE WAY A FINAL VERSION MAY WORK. WE ALSO MAY NOT RELEASE A FINAL
> VERSION OF SUCH FEATURES OR CONCEPTS. YOUR EXPERIENCE WITH USING SUCH
> FEATURES AND FUNCITONALITY IN A PHYSICAL ENVIRONMENT MAY ALSO BE
> DIFFERENT.
>
> **FEEDBACK**. If you give feedback about the technology features,
> functionality and/or concepts described in this demo/lab to Microsoft,
> you give to Microsoft, without charge, the right to use, share and
> commercialize your feedback in any way and for any purpose. You also
> give to third parties, without charge, any patent rights needed for
> their products, technologies and services to use or interface with any
> specific parts of a Microsoft software or service that includes the
> feedback. You will not give feedback that is subject to a license that
> requires Microsoft to license its software or documentation to third
> parties because we include your feedback in them. These rights survive
> this agreement.
>
> MICROSOFT CORPORATION HEREBY DISCLAIMS ALL WARRANTIES AND CONDITIONS
> WITH REGARD TO THE DEMO/LAB, INCLUDING ALL WARRANTIES AND CONDITIONS
> OF MERCHANTABILITY, WHETHER EXPRESS, IMPLIED OR STATUTORY, FITNESS FOR
> A PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT. MICROSOFT DOES NOT
> MAKE ANY ASSURANCES OR REPRESENTATIONS WITH REGARD TO THE ACCURACY OF
> THE RESULTS, OUTPUT THAT DERIVES FROM USE OF DEMO/ LAB, OR SUITABILITY
> OF THE INFORMATION CONTAINED IN THE DEMO/LAB FOR ANY PURPOSE.
>
> **DISCLAIMER**
>
> This demo/lab contains only a portion of new features and enhancements
> in Microsoft Power BI. Some of the features might change in future
> releases of the product. In this demo/lab, you will learn about some,
> but not all, new features.
