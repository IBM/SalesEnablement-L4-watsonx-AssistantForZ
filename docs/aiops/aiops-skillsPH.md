# Leveraging AIOps skills for assistant actions

In this section, you will explore the process of importing the **pre-packaged AIOps skills** that ship with the product and leveraging them as **Assistant actions** to provide operational insights into a z/OS system through an AI conversation with the Assistant. As an example, you can query a question about the health of your system and the assistant will return environment specific information about your running system. 

## placeholder

-provide details of the 47 different AIOps skills from Seismic
-link to the skill descriptions (Seismic/Github)
-Note that it won't be targeting the Wazi aaS back-end system from TechZone, but rather a "real" z/OS back-end for the purpose of demoing the flow 

## Importing the AIOps skills

-For the purpose of building your Pilot/Sandbox environment on TechZone, the JSON file with all the pre-packaged skills are provided below. The JSON file is fully configured and all you must do is import the file via Skills Studio. 

-Download the AIOps skills JSON file from here in Box:

-Go to Orchestrate, screenshots, etc.



## Adding to your personal skills and testing outputs

-provide the bearer token

## Connecting assistant to your app

## Building guided actions for your AIOps assistant

The IBM Documentation details the specific steps for configuring assistant action flows: https://www.ibm.com/docs/en/watsonx/waz/2.x?topic=assistants-building-actions-aiops-assistant-flows

Each subsection details the steps for configuring certain action flows, for example, the **LPAR flow**, **Db2 subsystems flow**, etc. 

Please reference the official IBM documentation 

This section will detail the steps to create the **LPAR flow** as detailed here (in the IBM Documentation). Please ensure to reference the IBM documentation when setting up the remainder of the flows. 

However, the steps involved to setup the different flows are similar:

1. Creating session variables (link to that tab in docs page) for that particular action flow as detailed here: 

2. Importing the necessary skills as **skill-based actions** within your assistant

3. Creating a **custom-built** action to guide users through the flow. 

## Building out the LPAR flow

The **LPAR flow** is a conversation that begins with a user input that starts the action, for example, ***"Show all my LPARs"***. The conversation continues as the AIOps assistant gathers more information and ends when the AIOps complestes the requested task or answers the customer's question. 

### Creating the session variables for LPAR flow

Once you've imported the AIOps skills into your watsonx Orchestrate environment and connected the app to your assistant, the first step in creating the LPAR flow is to define the relevant session variables which you can find here: https://www.ibm.com/docs/en/watsonx/waz/2.x?topic=flows-creating-variables

Note that within the **Variables table** on the page above, there are 7 specific **LPAR variables** that you will need to define when building out the **LPAR flow**. Ensure that you're referencing this table when building out the other subsystem flows to make available the necessary variables to your assistant. 

For the **LPAR flow**, there are 7 variables you will need to define:

- allLPARTable (type: Any)
- LPARNormalizedTable1 (type: Any)
- LPARNormalizedTable2 (type: Any)	
- LPARIdList (type: Free text)
- selectedLPARId (type: Free text)
- selectedLPARIdIndex (type: Number)
- selectedSMFId	(type: Free text)

1. Navigate to **AI assistant builder** 
   
   ![](_attachments/aiops1.png)

2. Select the **Actions** page from the left-hand menu
   
   ![](_attachments/aiops2.png)

3. On the **Actions** page, click on **Created by you** under the **Variables** section
   
   ![](_attachments/aiops3.png)

4. In the **Variables** window, click on **New variable +** in the top-right corner. 
   
   ![](_attachments/aiops4.png)

5. You will now add the first variable for the **LPAR flow** specified above, called ***allLPARTable*** with ***type: Any***. 
   
   In the **Session variable** window, enter the following **Name** and **Type** in the relevant fields. Leave all other defaults. 

   ***Name:***
   ```
   allLPARTable
   ```

   ***Type:***
   ```
   Any
   ```

   Once done, the variable window should look like the following:

   ![](_attachments/aiops5.png)

6. Click **Save** to save the new variable. 
   
   ![](_attachments/aiops6.png)

   Once saved, you should now see your first variable created in the list:

   ![](_attachments/aiops7.png)

7. Now **repeat steps 4 - 6** for the remaining 6 variables listed above. 
   
   When finished, your variables list should look like the following, containing all 7 variables with their respective variable **types**:

   ![](_attachments/aiops8.png)

### Creating the skills-based actions for LPAR flow

Once you have the **session variables** defined as described above for the **LPAR flow**, the next step is to define **skill-based actions** using the relevant skills for the **LPAR flow**. 

The three skills that are relevant to the **LPAR flow** which you will need to create **skills-based actions** for are:

- Returns information about all z/OS systems
- Retrieve a z/OS-system by LPAR name
- Retrieve jobs running on a z/OS system

For the other AIOps assistant flows, you would be using a different set of skills that you previously imported. But the above 3 are the ones that are needed to build out the **LPAR flow**. 

1. First, create a new actions folder/collection...
