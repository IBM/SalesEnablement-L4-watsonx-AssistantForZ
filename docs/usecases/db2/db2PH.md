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

9.  **Challenge**: You also need to add the **Retrieve job output** and **Retrieve job status** utility skills to your **db2-error** app just like you did when creating the **Gather Facts** skill flow. Repeat steps 2 - 8 to add the **Retrieve job output** and **Retrieve job status** utility skills to your **db2-error** app. 


## Verify all the skills are successfully imported and create the app connection.

1. Open **Skill catalog** in watsonx Orchestrate.

    ![](_attachments/c-skill-9a.png)

2. Enter **db2-error** in the search bar.

    **Enter screenshot**

3. Click the **db2-error** tile.

    **Enter screenshot**

4. Click **Add skill +** for each of the 3 skills in the certs app.

    **Enter screenshot**

5. Click **Connect app**.

    **Enter screenshot**

6. Enter your **AAP Username** and **AAP Password** and then click **Connect app**.

    **Enter screenshot**

7. Verify that the app is connected.

    **Enter screenshot**

## Connect the app to the assistant.

1. Open **Skill sets** in watsonx Orchestrate.

    ![](_attachments/c-app-1.png)

2. Select the **Draft** version of your assistant and click **Connections**.

    ![](_attachments/c-app-2.png)

3. Enter **db2-error** in the search bar.

    **Enter screenshot**

4. Click the ellipses (![](../_attachments/ellipsesIcon.png)) for the *db2-error* app and select **Connect app**.

    **Enter screenshot**

5. Click **Connect app**.

    **Enter screenshot**

6. Enter your **AAP Username** and **AAP Password** and then click **Connect app**.

    **Enter screenshot**