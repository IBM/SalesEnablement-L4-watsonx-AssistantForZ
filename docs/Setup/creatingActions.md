# Creating actions for your assistant
Once the skills in your application are connected to your assistant, you’re ready to begin creating actions tied to those skills. Learn more about building actions <a href="https://www.ibm.com/docs/en/watsonx/watson-orchestrate/current?topic=assistants-building-your-ai-assistant-actions" target="_blank">here</a>

# Configure the number of input fields
Before configuring actions, it’s important to modify a setting within watsonx Orchestrate that allows triggered skills to display as forms (versus conversational skills). 

1. Click your (a) profile icon and then click (b) **Settings**

    Learn more about configuring input fields <a href="https://www.ibm.com/docs/en/watsonx/watson-orchestrate/current?topic=actions-defining-how-interact-skill-in-conversation#configuring-multi-turn-conversations" target="_blank">here</a>.

    ![](_attachments/skillsConfig0.png)

2. Click the **Skill configurations** tab.

    ![](_attachments/skillsConfig1.png)

3. Enter **0** for the **Number of form fields**.

    ![](_attachments/skillsConfig2.png)

## Create actions
4. Click the main menu and select **AI assistant builder**.

    ![](_attachments/createActions0.png)

5. Hover over the **Home** icon (![](_attachments/homeIcon.png)) and click **Actions**.

    ![](_attachments/createActions1.png)

6. Click **Create action**.

    ![](_attachments/createActions2.png)

7. Click the **Skill-based action** tile.

    ![](_attachments/createActions3.png)

8. Select the **z/OS Gather Facts** tile and click **Next**.

    Note, it may take a minute for the page to display the action tiles. The date shown in the **z/OS Gather Facts** tile reflects when you added the skill to your application.

    ![](_attachments/createActions4.png)

9. On the **New action** dialog, (a) enter a prompt a user of the assistant might use to initiate the action and then (b) click **Save**.

    Sample prompts:

    ```
    Get z/OS facts
    ```

    ```
    Gather z/OS facts
    ```

    ![](_attachments/createActions5.png)

10. Add any (a) additional prompts and then (b) click the save (![](_attachments/diskIcon.png)).

    ![](_attachments/createActions6.png)

11. Click **Preview**.

    ![](_attachments/createActions7.png)

12. Enter one of the prompts you specified in step 9 or 10.

    Prompt:
    ```
    Get z/OS facts
    ```

    ![](_attachments/createActions8.png)

13. Review the returned results.

    ![](_attachments/createActions9.png)

    In the execution of this skill-based action, the skill executed properly and the output is the job id. If an error is generated, review the Troubleshooting section below.

# Verify the job in the Ansible Automation Platform console
Return to the Ansible Automation Platform (AAP) console and review the job information.

1. Click **Jobs** and expand the **## - z/OS Gather Facts** job.

    ![](_attachments/createActions10.png)

As seen in the assistant, the actual contents of the output aren’t displayed. The utility skills are used to retrieve the job output. It is also possible to create a skill flow that executes the **z/OS Gather Facts** skill followed by the **Retrieve job output** utility skill in sequence; passing the job id from the first skill to the second, in order to view the output within the assistant. Creating a skill flow is covered in the next section.

## Troubleshooting

??? Failure "Skill returns "***Sorry, we're having issues generating a response***"."

    ![](_attachments/skill-error-1.png)

    This error appears to be an intermittent issue when a skill is first added. To resolve, add the skill to your personal skills catalog using the steps that follow. If you encounter the issue, try the steps that follow:

    1. Expand the main menu and select **Chat**.
   
        ![](_attachments/skill-error-2.png)

    2. Click **Add skills from the catalog**.
   
        ![](_attachments/skill-error-7.png)

    3. Search for the skill app you created earlier and click the tile for your app.
   
        ![](_attachments/skill-error-8.png)
    
    4. Click **Add skill** for all the skills you want to add.

        ![](_attachments/skill-error-9.png)

    5. Click **Connect app**.
   
           ![](_attachments/skill-error-10.png)

    6. Enter the (a) **username** and (b) **password** using the username (admin) and password for your IBM Technology Zone (ITZ) watsonx Assistant for Z Pilot - AAP & z/OS reservation (AAP User Password (Use SSH key to login, only use password for UI)), and then click **Connect app**.

        ![](_attachments/skill-error-11.png) 

    7. Expand the main menu and select **Chat**.

        ![](_attachments/skill-error-12.png) 
    
    8. Try one of the prompts you created for your skill.

        Prompt:
        ```
        Gather z/OS facts
        ```

        ![](_attachments/skill-error-13.png) 

    You should now be able to run the skill through the assistant preview.