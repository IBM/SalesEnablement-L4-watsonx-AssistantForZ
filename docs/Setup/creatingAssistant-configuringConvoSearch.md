# Creating an assistant and configuring conversational search
To create your watsonx Assistant for Z, use [watsonx Orchestrate](https://www.ibm.com/products/watsonx-orchestrate?p1=Search&p4=43700077722754881&p5=e&p9=58700008198244496&gad_source=1&gclsrc=ds) to create the assistant and configure conversational search. You can configure your assistant to use conversational search by using a hosted [OpenSearch](https://opensearch.org/) instance. The pre-configured instance in IBM Technology Zone (ITZ) has over 220 knowledge sources and supports Retrieval Augmented Generation (RAG). The large language model (LLM) providing the conversational AI augments this knowledge based on IBM Z documentation. All of these elements create IBM Z context-aware responses to queries with its content-grounded knowledge.

## Access the ITZ IBM Cloud account for the watsonx Assistant for Z Pilot environment
1. In the IBM Technology Zone portal, expand **My TechZone** and select **My Reservations**, or click the following link.

     <a href="https://techzone.ibm.com/my/reservations" target="_blank">**ITZ My reservations**</a>
   
    ![](_attachments/itzMyReservations0.png)

2. Click the **watsonx Assistant for Z Pilot - watsonx Orchestrate** tile.

    ![](_attachments/itzMyReservations1.png)

3. Record the ITZ IBM Cloud account name associated with the reservation.

    ![](_attachments/itzMyReservations2.png)

4. Click the **IBM Cloud Login** link.

    ![](_attachments/itzMyReservations3.png)

    !!! Note "Steps to authenticate to IBM Cloud are not illustrated here."

        You may need to authenticate to IBM Cloud after clicking the link. These steps are not shown here as they may vary by individual.

5. Verify that the current IBM Cloud account is the same as the account name recorded in step 3. If the account is not the same, switch to the proper account.

    Note: the formatting of the name can appear differently than what is shown in the ITZ reservation.

    ![](_attachments/itzMyReservations4.png)

    If the proper account is not listed, click the account drop down and select the proper account. Note: if your browser window is narrow, the account drop down can be depicted with the Switch Account icon (![](_attachments/itzCloudChangeAccountIcon.png)).

    ![](_attachments/itzSwitchAccounts0.png)

## Create your Assistant
6. Click the **Resources** icon (![](_attachments/cloudResourcesIcon.png)).

    ![](_attachments/cloudResourcesMenu.png)

7. Expand the **AI / Machine Learning** section and click the **watsonx Orchestrate** instance listed (the instance name is different than shown in the following image).

    ![](_attachments/cloudResourcesAI.png)

8. Click **Launch watsonx Orchestrate**.

    ![](_attachments/cloudWOButton.png)

9. Click the **AI assistant builder** tile to start creating a new assistant.

    ![](_attachments/cloudWOAIAssistantBuilder.png)

10. Enter a name and optional description for your assistant and click **Next**.

    ![](_attachments/createAssistant1.png)

11. Complete the **Personalize your assistant** form and click **Next**.

    Explore the personalization options. In creating an assistant for a client pilot, consider specifying attributes that align with the client's business.

    **a**. Select **Web**.

    **b**. Select the industry of your choice.

    **c**. Select the role of your choice.

    **d**. Select the need of your choice.

    ![](_attachments/createAssistant2.png)

12. Complete the **Customize your chat UI** form and click **Next**.

    Explore the customization options. When creating an assistant for a client pilot, consider specifying attributes that align with the client (for example, colors and logos).

    ![](_attachments/createAssistant3.png)

13. Preview your assistant and then click **Create**.

    ![](_attachments/createAssistant4.png)

The assistant is now created.

![](_attachments/assistantHomePage.png)
<a name="configureCustomSearchURL"></a>
## Configure conversational search
The next step will be to configure **conversational search** for your assistant that uses a hosted instance of OpenSearch.

14. Click **Generative AI** menu item (![](_attachments/GenAIIcon.png)) in the left navigation.

    ![](_attachments/genAIMenu.png)

15. Review the base large language model (LLM) settings.

    Notice the other LLM models available. For most pilots, the **granite-3-8b-instruct** model is appropriate.

    ![](_attachments/genAILLMdefaults.png)

16. Click **Set up your Search Integration**.

    By default, conversational search is not enabled when an assistant is created. Conversational search takes priority over general-purpose answering if both are enabled. Learn more about conversational search in watsonx <a href="https://www.ibm.com/docs/en/watsonx/watson-orchestrate/current?topic=assistants-conversational-search" target="_blank">here</a>.

    ![](_attachments/genAILLMdefaults2.png)

17. Click **Custom service**.

    ![](_attachments/genAISetupCS1.png)

18. Complete the **Custom service** form and then (d) click **Next**.

    **a**. Select **By providing credentials**.

    **b**. Enter the following value in the **URL** field (use the copy icon to avoid typographical errors). This is the URL for the a shared [OpenSearch](https://opensearch.org/) instance. In later sections you will created and customize a dedicated instance.
    ```
    {{itz.hostedOpenSearchInstance}}
    ```

    **c**. Select **None** in the **Choose an authentication type** drop-down list.

    ![](_attachments/genAISetupCS2.png)

19. Enable **conversational search** and then click **Save**.

    ![](_attachments/genAISetupCS3.png)
       
20. Update the conversational search **custom service** settings based on your requirements.

    **Note**: the **Settings** page is divided into two sections in the following images to enhance the visibility of the screen captures. Learn more about these settings <a href="https://cloud.ibm.com/docs/watson-assistant/watson-assistant?topic=watson-assistant-conversational-search#tuning-the-generated-response-length-in-conversational-search" target="_blank">here</a>.

    The following settings are proven to work well. You can experiment with these settings to see how they affect queries for your client's pilot.

    **a**. Enable **Conversational search**.

    **b**. Select **Single turn**. Enabling multi-turn conversation (by selecting Entire conversation) is not yet supported for the solution on-premises. Be mindful in using this option and help ensure that the client understands what is supported in the solution.
    
    **c**. Specify the text that appears to instruct the user to expand the list of citations in the assistant (except web chat client).

    **d**. Select **Rarely** for the tendency to say "I don't know" setting.

    **e**. Select **Verbose** for the generated response length. This setting affects the average response length. Depending on user input, variations from the selected length can occur.

    ![](_attachments/genAISettings1.png)

    **f**. Leave the **Default filter** field empty.

    **g**. The **Metadata** field provides a way to adjust your assistant’s behavior during conversational search for your OpenSearch instance. This option is explored in detail in the [Installing and using zassist to ingest client documents](./zassist.md). Leave the field empty for now.

    **h**. The **Search display text** options specify the default text displayed when no results are found or when connectivity issues to the backend search service occur. You can keep the defaults or customize the service.

    ![](_attachments/genAISettings2.png)

21. Click (a) **Save** and then click (b) **Close**.

    ![](_attachments/genAISettings3.png)
## More configurations
After you save and close the **Conversational search** configuration page, a few more configurations are needed to get the best experience from your conversational chat. Details on these settings are available <a href="https://www.ibm.com/docs/en/watsonx/waz/2.x?topic=cluster-configuring-your-assistant-use-byos" target="_blank">here</a>.

22. Hover over the **Generative AI** icon (![](_attachments/GenAIIcon.png)) in the left navigation and click **Actions**.

    ![](_attachments/genAIActionsMenu.png)

23. Click **Set by assistant** under the **All items** menu.

    ![](_attachments/genAIActionsSetByAssistantMenu.png)

24. Click **No matches**.

    ![](_attachments/genAIActionsSetByAssistantMenu2.png)

25. Click **Step 1** under **Conversation steps**.

    ![](_attachments/genAIActionsSetByAssistantMenu3.png)

26. Select (**a**) **without conditions** in the **Is taken** drop-down menu and then click (**b**) **Clear conditions**.

    Note, the **Is taken** value does not change from **with conditions** after selecting **without conditions**.

    ![](_attachments/genAIActionsSetByAssistantMenu4.png)

27. Delete the default text in the **Assistant says** entry field.

    ![](_attachments/genAIActionsSetByAssistantMenu5.png)

28. Expand the **And then** drop-down menu and select **Search for the answer**.

    ![](_attachments/genAIActionsSetByAssistantMenu6.png)

29. Click **Edit settings**.

    ![](_attachments/genAIActionsSetByAssistantMenu7.png)

30. Select **End the actions after returning results** and then click **Apply**.

    ![](_attachments/genAIActionsSetByAssistantMenu8.png)

31. Click the **Save** (![](_attachments/diskIcon.png)) icon.

    ![](_attachments/genAIActionsSetByAssistantMenu9-b.png)

32. Select **Step 2** (**No matches count**) under **Conversation steps** and click the delete icon (![](_attachments/deleteIcon.png)).

    ![](_attachments/genAIActionsSetByAssistantMenu9.png)

33. Click **Delete** in the confirmation dialog to delete step 2.

    ![](_attachments/genAIActionsSetByAssistantMenu10.png)

34. Click the **x** to close the **Editor** window.

    ![](_attachments/genAIActionsSetByAssistantMenu11.png)

35. Click **Fallback** in the **Actions** table.

    ![](_attachments/genAIActionsSetByAssistantMenu12.png)

36. Delete **all** of the **Conversation steps**.

    Note: the following image is edited. Only 5 steps are shown, but all 6 need to be deleted. You need to select each step individually, click the delete icon (![](_attachments/deleteIcon.png)), and confirm the deletion.

    ![](_attachments/genAIActionsSetByAssistantMenu13.png)

37. Verify that all **Conversation steps** are deleted and then click the **x** to close the **Editor** window.

    ![](_attachments/genAIActionsSetByAssistantMenu14.png)

38. Click the **global settings** (![](_attachments/globalSettingsIcon.png)).

    ![](_attachments/genAIGlobalSettings1.png)

39. Click **No matches** under the **Conversation routing** tab.

    ![](_attachments/genAIGlobalSettings2.png)

40. Move the slider to **More often** (or select **More often** in the drop-down).

    The setting helps ensure that actions are triggered less often unless the user’s query specifically matches the action’s input.

    ![](_attachments/genAIGlobalSettings3.png)

41. Click **Autocorrection**.

    ![](_attachments/genAIGlobalSettings4.png)

42. Click the **autocorrection** toggle to turn the feature **off**.
    
    ![](_attachments/genAIGlobalSettings5.png)

43. Click (a) **Save** and then (b) **Close**.

    ![](_attachments/genAIGlobalSettings6.png)

44. Hover over the **home** (![](_attachments/homeIcon.png)) and click **Environments**.

    ![](_attachments/genAIEnviroments1.png)

45. Click **Web chat**.

    ![](_attachments/genAIEnviroments2.png)

46. On the **Style** tab, click the **Streaming** toggle to enable streaming.

    ![](_attachments/genAIEnviroments3.png)

47. Click **Suggestions**.

    ![](_attachments/genAIEnviroments4.png)

48. Click the **Suggestions** toggle to turn this feature **off**.
    
    ![](_attachments/genAIEnviroments5.png)

49. Click **Save and exit**.

    ![](_attachments/genAIEnviroments6.png)

## Configure the base large language model
After the preceding steps are completed, there are enhancements that can be made to configure how the large language model (LLM) responds to your queries. Including adding prompt instructions and configuring the LLM’s answer behavior.
These options can be summarized <a href="https://www.ibm.com/docs/en/watsonx/waz/2.x?topic=assistant-configuring-base-llm" target="_blank">here</a>.

1.  Hover over the **home** (![](_attachments/homeIcon.png)) and click **Generative AI**.
    
    ![](_attachments/genAIGenAI1.png)

2.  Click **Add instructions**.

    ![](_attachments/genAIGenAI2.png)

3.  Enter a **prompt instruction**.

    This option instructs the LLM in your assistant to give refined responses by adding prompt instructions. The instructions help the LLM guide the conversations with clarify and specifically to achieve the end-goal of an action.

    Enter the prompt instructions in the field. The maximum number of characters you can enter in the prompt instruction field is 1,000. 
        
    The following is an example prompt instruction that works well. Experiment with different prompt instructions.

    ```
    You are a subject matter expert on mainframe systems. Please respond to all prompts with truth and accuracy. Keep all answers short and concise, unless requested to provide details.
    ```

    **Note: When the instructions are typed in, they are automatically saved and the LLM is immediately trained on them.**

    ![](_attachments/genAIGenAI3.png)

4.  Toggle **General-purpose answering** to **off**.

    The ability exists to configure the answering behavior of your assistant to provide responses that are based on the preinstalled content or general content.

    On the **Generative AI** page (under **Prompt Instructions**), you see the **Answer behavior** section. After you configure **Conversational search**, you see that it is enabled (toggled on) with the search integration added.

    If you enable both General-purpose answering as well as Conversational search, the Conversational search answering takes precedence over General-purpose answering. 
    
    **For purposes of retrieving Z-specific answers and responses, it is recommended that you turn off General-purpose answering and leave only Conversational search turned on.**

    ![](_attachments/genAIGenAI4.png)

## Testing conversational search
Now, you can begin issuing queries to test the assistant's responses.

It is important to keep in mind that many of the settings configured earlier can be iteratively modified based on your assessment of the quality of responses. The settings can be reviewed and changed at any time. For example, adding extra prompt instructions, changing the verbosity of the responses, and modifying the indexes used for OpenSearch.

53. Hover over the **home** (![](_attachments/homeIcon.png)) and click **Preview**.

    ![](_attachments/preview0.png)

54. Experiment with different prompts and validate the answers are reasonable and related to IBM Z.

    Other prompts and responses follow. Note: the responses that you receive can vary from the ones shown.

    **Prompt**: 

    ```
    What is the APF list in z/OS? Provide a detailed explanation?
    ```

    **Example output**:

    ![](_attachments/preview1.png)

    **Prompt**:

    ```
    Why is Db2 different than other database systems?
    ```

    **Example output**:

    ![](_attachments/preview2.png)

    **Prompt**:

    ```
    What happens during an IPL on IBM Z?
    ```

    **Example output**:

    ![](_attachments/preview3.png)

<!-- Should we add a query that will eventually fire a skill off and show that id doesn't do that right now? -->

You now have a working assistant that uses {{offering.name}}. Take time to explore different prompt instructions and settings. If you encounter any issues, the Troubleshooting section that follows can help resolve them.

Continue to the [Creating a stand-alone OpenSearch instance for document ingestion](documentIngestion.md) learn how to configure a dedicated OpenSearch instance for ingesting client-specific documentation into the RAG model.

## Troubleshooting
The following are issues that you may encounter. If the provided resolutions do not work, contact support by using the methods that are mentioned in the [Support](../index.md#support) section.

??? Failure "Assistant responds to all prompts with, "I might have information related to your query to share, but am unable to connect to my knowledge base at the moment""

    This Assistant is unable to connect to the custom service URL specified. This could be a network issue, the service may be down, the service may be restarting, or the service is no longer running at that URL.

    Before reaching out to [Support](../index.md#support), try the following:

    - Wait a few minutes and try again. It may be the service was in the process of restarting.
        
    - If you printed this demonstration guide or saved a copy, verify you are using the most current version of the <a href="{{guide.url}}" target="_blank">lab guide</a> and the correct service URL ({{itz.hostedOpenSearchInstance}}). The URL may have changed since you saved or printed the lab guide.

