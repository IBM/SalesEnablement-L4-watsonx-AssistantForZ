# Creating skill flows
As seen in the previous section, running the Ansible skill to **Gather z/OS facts**, the skill executed successfully and was verified within the Ansible Automation Platform (AAP) console by viewing the job output. However, the output wasn’t displayed by the assistant. To enable this scenario, a skill flow is needed. Skills are often more valuable when combined with other skills. You can create a skill flow to use two or more skills together to finish a task (like returning the output of a previous skill). When you create a skill flow, you map the output of one skill as the input for subsequent skills. Learn more about creating skill flows <a href="https://www.ibm.com/docs/en/watsonx/waz/2.x?topic=combining-skills-into-skill-flows" target="_blank">here</a>.

As mentioned in a previous section, there are some default utility skills that are provided out of the box with the “Z Skills Accelerator” which are leveraged to return the output of a skill. To accomplish this, we will import the Ansible Utility skill called **Retrieve job output**.

# Add the utility skill
1. Open IBM watsonx Orchestrate **Skill studio**.

    ![](_attachments/skillFlow0.png)

2. Expand **Create** and click **Import API**.

    ![](_attachments/skillFlow1.png)

3. Click the **z/OS Skills accelerator (Trial)** tile.

    ![](_attachments/skillFlow2.png)

4. Enter the following values in the **z/OS Skills accelerator** form and then click **Connect**.

    Use the **URL**, **User Name**, and **Password** values recorded in the [Explore Ansible Automation Platform](exploreAAP.md) section earlier.

    **a**: Connection Type: **ansible**

    **b**: Application Name: <use the same application name as in previous section\> - *

    **c**: Connection URL: <enter the URL for your AAP UI\>

    **d**: User Name: <enter the AAP User Name (for UI access)\>

    **e**: Password: <enter the AAP User Password\>

    **f**: Search Pattern: **\***

    ![](_attachments/skillsForm.png)

5. Expand **Ansible Utility Skills** and click **Ansible Utility Skills**.

    ![](_attachments/skillFlow3.png)

6. Select **Retrieve job output** and click **Save as draft**.

    ![](_attachments/skillFlow4.png)

7. Click the ellipses (![](_attachments/ellipsesIcon.png)) for the **Retrieve job output** skill and select **Enhance this skill**.

    ![](_attachments/skillFlow5.png)

8. Review the skill settings and then click **Publish**.

    ![](_attachments/skillFlow6.png)

9. Select **Skill sets** from the main menu.

    ![](_attachments/skillFlow7.png)

10. Select (a) your draft assistant in the **Team Skills** drop-down list and (b) click the **Connections** tab.

    ![](_attachments/skillFlow8.png)

11. Click the **Search** (![](_attachments/searchIcon.png)) icon.

    ![](_attachments/skillSets3.png)

12. Search for the application name you specified earlier.

    ![](_attachments/skillSets4.png)

13. Click the (a) ellipses (![](_attachments/ellipsesIcon.png)) for your application and (b) click **Edit connection**.

    ![](_attachments/skillFlow9.png)

14. Verify the application is (a) **Connected** and then (b) click the **x** to close the dialog.

    ![](_attachments/skillFlow10.png)

## Add the skills to your Personal skills
15. Click **Skill catalog** in the main menu.

    ![](_attachments/skillFlow11.png)

16. Search for the application name you specified earlier.

    ![](_attachments/skillFlow12.png)

17. Click the tile for your application.

    Note, the tile name is proceeded by **Ansible Controller Skills**.

    ![](_attachments/skillFlow13.png)

18. Click **Add skill** for each of the skills you want to add to the flow.

    ![](_attachments/skillFlow14.png)

## Create the skill flow
19. Click **Skill studio** in the main menu.

    ![](_attachments/skillFlow15.png)

20. Expand the **Create** drop-down menu and click on **Skill flow**.

    ![](_attachments/skillFlow16.png)

21. Click the **+** icon.

    ![](_attachments/skillFlow17.png)

Next, you need to add the **z/OS Gather Facts** skill and the **Retrieve job output** skill to the skill flow. Use the **Search apps** function to locate the skills.

22. Search for the application name you specified earlier and click it's tile.

    ![](_attachments/skillFlow18.png)

23. Click **Add Skill** in the **z/OS Gather Facts** tile.

    ![](_attachments/skillFlow19.png)

24. Verify the **z/OS Gather Facts** skill is added to the skill flow.

    ![](_attachments/skillFlow20.png)

25. Click the **+** icon **after** the **z/OS Gather Facts** tile.

    ![](_attachments/skillFlow21.png)

26. Repeat steps 22 and 23 for the **Retrieve job output** skill. 

    After adding the **Retrieve job output** skill, your skill flow should like like:

    ![](_attachments/skillFlow22.png)

## Map the outputs and inputs of the skills
Next you must map the output values of the first skill to the input of the second skill. In this case, pass the “job id” output from **z/OS Gather Facts** as an input for **Retrieve job output**. 

27. Click the **Retrieve job output** tile.

    ![](_attachments/skillFlow23.png)

28. Select the **Input** tab and click in the **id** field.

    ![](_attachments/skillFlow24.png)

29. Click the **z/OS Gather Facts** skill in the **Mapping data for "id"** section.

    ![](_attachments/skillFlow25.png)

30. Optionally, toggle the **Hide this from from the user** setting.

    For this lab guide, this option is left disabled. Learn more about this option <a href="https://www.ibm.com/docs/en/watsonx/waz/2.x?topic=combining-skills-into-skill-flows#hiding-input-and-output-forms" target="_blank">here</a>.

    ![](_attachments/skillFlow26.png)

31. Click the **x** to close mapping window.

    ![](_attachments/skillFlow27.png)

32. Click the pencil (![](_attachments/pencilIcon.png)).

    ![](_attachments/skillFlow28.png)

33. Enter a (a) **Name** and (b) **Description** for your skill flow and then (c) click **Save**.

    ![](_attachments/skillFlow29.png)

34. Expand the **Actions** pull-down list and click **Save as draft**.

    ![](_attachments/skillFlow30.png)

35. Expand the **Actions** pull-down list and click **Enhance**.

    ![](_attachments/skillFlow31.png)

## Enhancing the skill flow
On the **Enhancing the skill** pages, you can:
- modify the skill name, description, and version
- add phrases (prompts) that will be recognized by the assistant to call the skill flow
- 

36. Click the **Phrases** tab.

    ![](_attachments/skillFlow32.png)

37. Enter new **phrases** (prompts) for your skill flow and then click **Publish**.

    Notice the default prompts are either not very intuitive (the skill flow name) or a bit verbose. Add a couple of other prompts that you anticipate users will enter.

    Example prompts:
    ```
    Show me z/OS facts
    ```
    ```
    Gather and display z/OS facts
    ```

    ![](_attachments/skillFlow33.png)

## Enable the skill flow in your assistant

38. Click **AI assistant builder** in the main menu.

    ![](_attachments/skillFlow34.png)

39. Hover over the **Home** (![](_attachments/homeIcon.png)) and click **Actions**.

    ![](_attachments/skillFlow35.png)

40. Click **New action**.

    ![](_attachments/skillFlow36.png)

41. Click the **Skill-based action** tile.

    ![](_attachments/skillFlow37.png)

42. Click the skill flow you created earlier and then click **Next**.

    ![](_attachments/skillFlow38.png)

43. Enter an example prompt for the skill and click **Save**.

    You can use one of the prompts you used earlier for the skill flow.

    ```
    Show me z/OS facts
    ```

    ![](_attachments/skillFlow39.png)

44. Enter any additional phrases (prompts) and then click the **save** (![](_attachments/diskIcon.png)).

    ![](_attachments/skillFlow40.png)

45. Click **Preview**.

    ![](_attachments/skillFlow41.png)

46. Enter one of the prompts you specified into the assistant preview.

    ![](_attachments/skillFlow42.png)

47. Click **Apply** when the first form is returned.

    ![](_attachments/skillFlow43.png)

48. Review the results from the skill flow.

    Use both scroll bars in the assistant preview to review all of the returned information. 

    ![](_attachments/skillFlow44.png)

The scenario shown above may or may not be relevant for your client's use case. It is intended to show how you to sequence skills together in a skill flow to create an action that your assistant triggers based on prompts using the pre-configured Ansible automation templates. You are encouraged to experiment with your own skill flows and prompts using other skills available within the AAP instance.

Next, learn about custom-built actions.