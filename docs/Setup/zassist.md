# Installing and using zassist to ingest client documents
With Bring Your Own Search (BYOS) installed and configured in your assistant, you can now prepare for document ingestion. Currently, only PDF, HTML, and DOCX file formats are supported for ingestion. 


To prepare for document ingestion, you can also reference the setup instructions located <a href="https://ibmdocs-test.dcs.ibm.com/docs/en/watsonx/waz/2.0?topic=install-zassist-ingest-data" target="_blank">here</a>.

## Install the zassist utility
The **zassist** utility is an executable program that automates the ingestion of client documentation into the RAG for watsxonx Assistant for Z. The utility is available to clients through <a href="https://www.ibm.com/software/passportadvantage/pao_customer.html" target="_blank">IBM Passport Advantage</a>.

A version of zassist is available for download for IBMers and Business Partners for conducting pilots. Follow the steps below to download and install **zassist**.

1. Click the link below and download the **zassist.zip** file.
   
    <a href="https://ibm.box.com/s/j3nt5iw4fqd5w2jgcqwxnjlsu8bpvl77" target="_blank">https://ibm.box.com/s/j3nt5iw4fqd5w2jgcqwxnjlsu8bpvl77</a>

    ![](_attachments/zassistDownload.png)

2. Unzip the **zassist.zip** file.
3. Locate the appropriate executable for your local machines operating system.

    ![](_attachments/zasstZIPFile.png)

4. Either copy the appropriate **zassist** executable to a directory in your command prompt or terminal window's PATH, or copy it to a directory and add that directory PATH to your PATH environment variable.

    Additional information for performing the above tasks can be found <a href="https://www.ibm.com/docs/en/watsonx/waz/2.x?topic=data-installing-zassist#tasktask_w13_lhf_4bc__steps__1" target="_blank">here</a>.

5. Verify the **zassist** utility is working.

    ![](_attachments/zassistRunning.png)

## Ingest client documentation using **zassist**
With the zassist executable ready, you are now able to begin ingesting data. 

Step-by-step guidance for ingesting documents using zassist are provided in the IBM watsonx Assistant for Z documentation <a href="https://www.ibm.com/docs/en/watsonx/waz/2.x?topic=data-ingesting" target="_blank">here</a>.

These steps are not repeated in this lab guide, rather the video below illustrates all of the steps to ingest a single document. This video has no audio.

![type:video](_videos/zassitIngest-final.mp4)

The document ingested in the video is a compressed PDF version of the **IBM z/OS Continuous Delivery** Red Piece. You can download a copy of this document <a href="https://github.com/IBM/SalesEnablement-L4-watsonx-AssistantForZ/blob/main/docs/Setup/_sampleDocs/redp5340-compressed.pdf" target="_blank">here</a>.

## Verify the document ingested is now returned as a source file for a query.
Use the watsonx Orchestrate AI assistant builder to verify your document ingestion.

6. Hover over the home (![](_attachments/homeIcon.png)) icon and click **Preview**.
7. Enter the following prompt in your assistant.

    ```
    What is z/OS continuous delivery?
    ```

    ![](_attachments/verifyIngest0.png)

8. Expand the sources section by clicking the (![](_attachments/downArrowIcon.png)).
   
    ![](_attachments/verifyIngest1.png)

9. Click through the list of resources and find the reference to the Red Piece document you ingested.

    ![](_attachments/verifyIngest2.png)

10. Click the ingested document reference.

    ![](_attachments/verifyIngest3.png)

11. Accept the security risk to view the source document.

    The steps to accept the security risk for the document are not shown. This occurs as the certificate for the connection to the SNO instance is not secure. Notice the URL contains the a path to your SNO instance route.

        ![](_attachments/verifyIngest4.png)

 ## Adjusting your assistant's search behavior
 Do you recall the **Metadata** field when you were configuring your assistant?

![](_attachments/genAISettings2.png)

The Metadata field provides a way to adjust your assistant’s behavior during conversational search for your OpenSearch instance. Now that you have your own docs ingested for conversational search, you can set the metadata field for your assistant to use those documents in it’s content-grounded search.
If you leave the metadata field empty, then it defaults to settings found to perform well experimentally. This replaces having to paste a complicated search string.
By default (without any string in the Metadata field), it will search all the default IBM provided documentation and all ingested customer documentation using the following:

```
{“ibm_indices”:“*_ibm_docs_slate”,
“customer_indices”:“customer_*”}
```

Replacing the wildcard string with an explicit list of indices allows personalization. This is where you could input specific indices (pointing to the underlying documentation) that you want your assistant to use for the content-grounded search. Out of the box there are over 220 products and topics which the OpenSearch instance has IBM documentation for. You can find those indices and products <a href="https://ibm.box.com/s/anioal2xuwbsck8v3l4r48juzh9tbcqn" target="_blank">here</a>.

You have the ability to input a subset of indices into the “Metadata” field in cases where you only want your assistant gathering context for specific IBM products or topics. The specific indices can be listed out in this format:

```
{“ibm_indices”:“<comma separated index values>”,“customer_indices”:“customer_*”}
```

For example, if you only want your assistant to reference documentation for “Db2 Analytics Accelerator for z/OS” and no ingested client documentation, you could enter the following into the metadata field:

```
{“ibm_indices”:“ss4lq8_ibm_docs_slate”}
```

If you have a mix of IBM documentation and client documentation ingested, then there’s an optional search string you can use to set the “weights” used for each. 

For example:

```
{"doc_weight":
{"product_docs":0.5,
 "customer_docs":0.5},
"ibm_indicies":"*_ibm_docs_slate",
"customer_indicies":"customer_*"
}
```

In this case, “product_docs” is the weight assigned to “ibm_indices” and “customer_docs” is the weight assigned to “customer_indices”.

Once you’ve configured all the above settings for Conversational Search on that page, you can click “Save” in the top-right of the page.

For more information on customizing the metadata field for conversational search, please refer to this supplemental video found <href="https://ibm.ent.box.com/s/2quy4drqp3bolgd6flqm0l1c549fz64x/file/1661645917984" target="_blank">here</a>.

You are encouraged to experiment with the metadata field!
Try setting the metadata field to the following which weights ingested docs higher than the product docs:

```
{"doc_weight":
{"product_docs":0.2,
 "customer_docs":0.8},
"ibm_indicies":"*_ibm_docs_slate",
"customer_indicies":"customer_*"
}
```

Now redo steps 6 through 8 above. Notice the ingested Red Piece document is now the first sited reference!

![](_attachments/verifyIngest5.png)
