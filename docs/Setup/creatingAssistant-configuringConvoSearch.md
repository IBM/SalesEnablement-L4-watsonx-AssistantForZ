# Creating an Assistant & configuring conversational search
This section will cover how to begin using [watsonx Orchestrate](https://www.ibm.com/products/watsonx-orchestrate?p1=Search&p4=43700077722754881&p5=e&p9=58700008198244496&gad_source=1&gclsrc=ds) to create a new assistant for watsonx Assistant for Z and configure conversational search. You will be able to configure your assistant to use conversational search using a hosted [OpenSearch](https://opensearch.org/) instance. The pre-configured instance in IBM Technology Zone (ITZ) has over 220 knowledge sources and supports the Retrieval Augmented Generation (RAG) in which the Large Language Model (LLM) providing the conversational AI is augmented by this knowledge based on IBM Z documentation. All of which helps create IBM Z context-aware responses to queries with its content-grounded knowledge.

<!-- Note: In this section, you can test setting up conversational search using a hosted OpenSearch instance in ITZ. If you would like to support a use case in which you’re able to ingest your own documentation in addition to the pre-packaged IBM Z Documentation, you will need to leverage Bring Your Own Search (BYOS). The following section after this is optional and provides guidance on how to do this, i.e. deploying your own OpenSearch instance on an OCP cluster. The instance will have the pre-installed IBM Z documentation along with a data ingestion service that you can use to load your own documents. You will then be able to configure watsonx Assistant for Z and its search parameters to fetch content from these documents. -->

## Create your Assistant
1. If not already open, click the link below to open a browser to the IBM Cloud portal and authenticate using your email address.

    <a href="cloud.ibm.com" target="_blank">**IBM Cloud portal**</a>

2. Click the account menu and select the {{itz.account1}} account.

    ![](_attachments/itzCloudSwitchAccounts.png)

3. Click the **resources** icon (![](_attachments/cloudResourcesIcon.png)).

    ![](_attachments/cloudResourcesMenu.png)

3. Expand the **AI / Machine Learning** section and click the **watsonx Orchestrate** instance listed (the instance name will be different than shown in the image below).

    ![](_attachments/cloudResourcesAI.png)

4. Click **Launch watsonx Orchestrate**.

    ![](_attachments/cloudWOButton.png)

5. Click the **AI assistant builder** tile to start creating a new assistant.

    ![](_attachments/cloudWOAIAssistantBuilder.png)

6. Enter a name for your assistant and click **Next**.

    ![](_attachments/createAssistant1.png)

7. Complete the **Personalize your assistant** form and click **Next**.

<!-- MAY WANT TO ADD INFO ON WHY THESE CHOICES OR HOW THEY ARE ACTUALLY USED DOWN THE ROAD -->

    **a**. Select **Web**.

    **b**. Select the industry of your choice.

    **c**. Select the role of your choice.

    **d**. Select the need of your choice.

    ![](_attachments/createAssistant2.png)

8. Complete the **Customize your chat UI** form and click **Next**.

    ![](_attachments/Zeeves.png)

    




## Troubleshooting

The following are issues you may encounter. If the provided resolutions do not work, contact support using the methods mentioned in the [Support](../index.md#support) section of this guide.

??? Failure "Assistant responds to all prompts with, "I might have information related to your query to share, but am unable to connect to my knowledge base at the moment""

    This Assistant is unable to connect to the custom service URL specified. This could be a network issue, the service may be down, the service may be restarting, or the service is no longer running at that URL.

    Before reaching out to support, try the following:

    - Wait a few minutes and try again. It may be the service was in the process of restarting.
        
    - If you printed this demonstration guide or saved a copy, verify you are using the most current version of the <a href="{{guide.url}}" target="_blank">lab guide</a> and the correct service URL ({{itz.hostedOpenSearchInstance}}). The URL may have changed since you saved or printed the lab guide.

