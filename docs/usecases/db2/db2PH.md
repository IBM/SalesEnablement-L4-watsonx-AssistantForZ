 # Use case: Db2 Error Message Debugging

 Database Administrators (DBAs) often spend significant time performing many routine database management tasks. And may often require manual intervention, increasing the risk of human error, slowing down operations and reducing overall productivity. And especially for early-tenure staff members, the troubleshooting and diagnosing error messages can take longer than necessary due to knowledge gaps, manual processes and lack of automation. 

 This scenario will show how a purpose-built assistant flow can help streamline the process of understanding a particular error message, generating accurate and current answers to questions, along with step-by-step guidance to learn more with conversational AI. Additionally, the assistant has the ability to recommend skills at different points in the conversation to help automate the resolution on their behalf, depending on the error message. This results in less time spent debugging error messages and improving the time-to-resolution of Db2 for z/OS errors.

 ## Test the Db2 Reorg Ansible Automation in AAP

 **placeholder**

 ## Importing the Ansible Playbooks as Skills

 For this use case, you will need to import 3 Ansible templates into watsonx Orchestrate as skills. 

 The Ansible templates you will import are:

 - Db2 Reorg
 - Retrieve job output (*utility skill*)
 - Retrieve job status (*utility skill*)

1. Open **Skill studio** in watsonx Orchestrate.
    
    ![](_attachments/c-skill-1a.png)

2. Click **Create** and then click **Import API**.

    ![](_attachments/c-skill-2a.png)

3. Click the **z/OS Skills accelerator (Trial)** tile.

    ![](_attachments/c-skill-3a.png)

4. Enter the following values in the **z/OS Skills accelerator** form and then click **Connect**.

    Use the **URL**, **User Name**, and **Password** values recorded in the [Explore Ansible Automation Platform](../../skills/exploreAAP.md) section earlier.

    **a**: Connection Type: 
    ```
    ansible
    ```

    **b**: Application Name: 
    ```
    db2-error
    ```

    **c**: Connection URL: 
    
    <Enter the URL for your AAP UI\>

    **d**: User Name: 
    
    <Enter the AAP User Name (for UI access)\>

    **e**: Password:
    
    <Enter the AAP User Password\>

    **f**: Search Pattern: 
    ```
    *
    ```

    ![](_attachments/db2-1.png)

    


5. Expand the **Ansible Job Template Proj...** folder and then click **aap4zos**.

    ![](_attachments/c-skill-5a.png)

6. Select **Db2 Reorg** and then click **Save as draft**.

    ![](_attachments/db-2.png)

7. Click the ellipses (![](../_attachments/ellipsesIcon.png)) for the **Db2 Reorg** skill and select **Enhance this skill**.

    ![](_attachments/db2-3.png)

8.  Review the skill enhancement options and then click **Publish**.

    ![](_attachments/db2-4.png)

9.  **Challenge**: You also need to import the **Retrieve job output** and **Retrieve job status** utility skills to your **db2-error** app just like you did when creating the **Gather Facts** skill flow. Repeat steps 2 - 8 to import the **Retrieve job output** and **Retrieve job status** utility skills to your **db2-error** app. 
    
    Once done, you should be able to see all 3 skills in your **Skills Catalog** in the **Published** state.


## Verify all the skills are successfully imported and create the app connection.

1. Open **Skill catalog** in watsonx Orchestrate.

    ![](_attachments/c-skill-9a.png)

2. Enter **db2-error** in the search bar and click the application tile..

    ![](_attachments/db2-5.png)

3. Click **Add skill +** for each of the 3 skills in the db2-error app and then click **Connect app**.

    ![](_attachments/db2-6.png)

4. Enter your **AAP Username** and **AAP Password** and then click **Connect app**.

    ![](_attachments/db2-7.png)

5. Verify that the app is connected.

    ![](_attachments/db2-8.png)

## Connect the app to the assistant.

1. Open **Skill sets** in watsonx Orchestrate.

    ![](_attachments/c-app-1.png)

2. Select the **Draft** version of your assistant and click **Connections**.

    ![](_attachments/c-app-2.png)

3. Enter **db2-error** in the search bar.

    ![](_attachments/db2-9.png)

4. Click the ellipses (![](../_attachments/ellipsesIcon.png)) for the *db2-error* app and select **Connect app**.

    ![](_attachments/db2-10.png)

5. Click **Connect app**.

    ![](_attachments/db2-11.png)

6. Enter your **AAP Username** and **AAP Password** and then click **Connect app**.

    ![](_attachments/db2-12.png)

## Create a skill flow to extract and display job completion details

**The goal of this...**

Recall in the AAP template you executed earlier the output that was displayed showing:

- asfads
- asdfa
- asdf

You will build a skill flow that extracts this information from the Ansible automation's output and formats and displays it to the end user.

1. Open **Skill studio** in watsonx Orchestrate. 
   
    ![](_attachments/c-skill-1a.png)
   
2. Click **Create** and then click **Skill flow**.

    ![](_attachments/c-sf-2a.png) 

3. Click the **+** icon.

    ![](_attachments/c-sf-3a.png) 

4. Click the **db2-error** app.

    !!! Tip "Search on **db2-error** if you do not see the tile for your app."

    ![](_attachments/db2-13.png)

5. Click **Add Skill +** in the **Retrieve job output** tile.

    ![](_attachments/db2-14.png)

6. Click the **+** icon below the **Retrieve job output** skill. Next you'll add the **Input form** skill to extract certain output details. 

    ![](_attachments/db2-15.png)

7. Click the **Custom forms** app tile. 
   
    ![](_attachments/db2-16.png)

8. Click **Add skill +** in the **Input form** tile. 
   
    ![](_attachments/db2-17.png)

9. **Repeat steps 6 - 8** to add the **Output form** skill after the previous skill from the same ***Custom forms*** app. The resulting skill flow should like the following:
    
    ![](_attachments/db2-18.png)


Now that you have the skill flow built out, you will modify the inputs/outputs to display useful information about the **Db2 Reorg** automation post-completion. 

10. Click on the **Input form** tile and enter a **Form title**, such as ***Job completion details***.
    
    ![](_attachments/db2-19.png)

11. Then click on **Add input field +**.
    
    ![](_attachments/db2-20.png)

12. Select **Paragraph text** and then click **Next**. 
    
    ![](_attachments/db2-21.png)

13. For this input field, enter a **Display label** of ***Return Code*** and then click **Apply**. 
    
    ![](_attachments/db2-22.png)

14. Click on the resulting text box for the **Return Code** field. 
    
    ![](_attachments/db2-23.png)

    Then in the  **Mapping data** window, select your **Retrieve job output** skill, and then click on the **content** skill output. 

    ![](_attachments/db2-24.png)

    The result should look like the following:

    ![](_attachments/db2-25.png)

15. Now **repeat steps 11 - 14** to add two more **Input fields** with **Display labels** titled:

    - Job ID
    - Image Copy DSN

    Once done, you should see the following with all 3 Input Fields:

    ![](_attachments/db2-26.png)