# Use case: SSL Certificate renewal on z/OS
Now, shift roles to that of a mainframe Security Administrator (SA). The client want to understand how watsonx Assistant for Z can help them to verify that their critical security certificates are up to date and reduce the risk of expired certificates disrupting their organization’s services. 

Secure Sockets Layer (SSL) certificates, often referred to as digital certificates, are used to establish an encrypted connection between communicating parties over a network. Certificate management is crucial for maintaining the security of a company’s z/OS environment. The SA has not performed the tasks to manage and renew a certificate in some time. The SA recalls that there are many steps that are required on z/OS and various RACF commands that need to be run to renew a certificate. Rather than going to their senior SA for assistance, demonstrate how using watsonx Assistant for Z can help the SA automate the certificate renewal process. 

In this scenario, use the Ansible automation templates that are provided with AAP and the WAZI z/OS environment to create assistant actions. The actions guide the client through the process of identifying their SSL certificate’s expiration dates, and automating the certificate renewal process for them. The assistant saves them time and improve their productivity.

## Create an initial certificate authority (CA) certificate to sign future SITE certificates
For this use case, a certificate authority (CA) certificate is needed to sign new SITE certificates. 

1. Open and log in to the Ansible Automation Platform (AAP) web console. 

    !!! Tip "Don't remember how?"
 
        Refer to the first 5 steps in [Explore Ansible Automation Platform](../../skills/exploreAAP.md).

2. Click **Templates** under the **Resources** section.

     ![](_attachments/cert-1a.png)
     
3. Click the **launch** icon (![](../_attachments/rocketIcon.png)) for the **z/OS Certs - Create Cert** template.

     ![](_attachments/cert-2a.png)

4. On the **Survey** screen, modify the **Certificate Label** and **Type** fields with the values that follow and then click **Next**. 

    **a**: **Certificate Label**
    ```
    TESTCA
    ```

    **b**: **Type**
    ```
    CERTAUTH
    ```

    !!! Note "Leave the default values for all other fields."

    ![](_attachments/cert-3a.png)    

5. Click **Launch**.

     ![](_attachments/cert-4a.png)   

6. Review the output of the job.

     In the output of the playbook, notice that a new keyring is created, a certificate is created, and the certificate is connected to the key ring.

     ![](_attachments/cert-5a.png)    

7. Locate the line **TASK [GENERATE new certificate]**, click the **changed: [zos host]**.

     ![](_attachments/cert-6a.png)   

8.  Click **JSON**.

     ![](_attachments/cert-7a.png)  

9.  Review the **RACDCERT** command that was run to generate the certificate and then click **x** to close the window.

     ![](_attachments/cert-8a.png)  
    
## Create an *expiring* certificate
Now, create an expiring certificate that uses the CA certificate that you just created.

1. Return to the **Templates** tab and click the **launch** icon (![](../_attachments/rocketIcon.png)) for the **z/OS Certs - Create Cert** template.

     ![](_attachments/cert-9a.png)  

2. On the **Survey** screen, modify the fields that follow with the values specified and then click **Next**. 

    **a**: **Type**
    ```
    SITE
    ```

    **b**: **Sign with**
    ```
    CERTAUTH
    ```

    **c**: **Sign Label**
    ```
    TESTCA
    ```

    **d**: **Common Name**
    ```
    company.com
    ```

    **e**: **Expiration Date**
    
    Enter a date that occurs within the next 30 days. The date must be in the format YYYY-MM-DD.

    !!! Note "Leave the default values for all other fields."

    Unlike the first certificate you created which was *self-signed*, this certificate will be signed by the local certificate authority that uses the CA you created.

    !!! Note "The following image does not highlight all the fields that need to be modified!"

    ![](_attachments/cert-10a.png)  

3. Click **Launch**.

     ![](_attachments/cert-11a.png)  

4. Verify that the job was successful and inspect the output of the job.

     ![](_attachments/cert-12a.png) 

## Renew the *expiring* certificate
Now that you have a certificate and it is expiring within 30 days, it is time to renew the certificate.

1. Return to the **Templates** tab and click the **launch** icon (![](../_attachments/rocketIcon.png)) for the **z/OS Certs - Search and Renew** template.

     ![](_attachments/cert-13a.png) 

2. On the **Survey** screen, modify the fields that follow with the values specified and then click **Next**.

    **a**: **Certificate Label**
    ```
    TESTSITE
    ```

    **b**: **Type**
    ```
    SITE
    ```

    **c**: **Sign with**
    ```
    CERTAUTH
    ```

    **d**: **Sign Label**
    ```
    TESTCA
    ```

    **e**: **Expiration Date**
    
    Specify a new expiration date in the format YYYY-MM-DD.

    !!! Note "The following image does not highlight all the fields that need to be modified!"

    ![](_attachments/cert-14a.png)  

3. Click **Launch**.

    ![](_attachments/cert-15a.png)  

4. Verify that the job was **Successful** and review the output.

    **Note**: Click the **Reload Output** button to view the full output after the job completes.

    Review the tasks that were run within the automation to renew the certificate. Some of the steps completed include:

    - Run the RACF_CERTIFICATE_EXPIRATION z/OS Health Check

    - Submit JCL to pull a report on the z/OS Health Check

    - Search the output of the report for the given certificate label

    - Print the expiring certificate, if it is found. You see: ‘TESTSITE expiring – True’

    - If the certificate is expiring, start a series of RACDCERT commands to do the following:
  
         - Backup the expiring certificate
       
         - Rekey the certificate and give it a new temporary label
       
         - Generate a CSR for the new certificate
       
         - Sign the new certificate with the local CA
       
         - Delete the old certificate
       
         - Relabel the new certificate to use the same label as before
       
         - Refresh the digital certificate list
  
     ![](_attachments/cert-16a.png) 

## Create another *expiring* certificate
Create one more expiring certificate to use with the assistant and the new skills you will create.

1. Return to the **Templates** tab and click the **launch** icon (![](../_attachments/rocketIcon.png)) for the **z/OS Certs - Create Cert** template.

     ![](_attachments/cert-17a.png) 

2. On the **Survey** screen, modify the fields that follow with the values specified and then click **Next**.

    **a**: **Certificate Label**
    ```
    DEMOCERT
    ```

    **b**: **Type**
    ```
    SITE
    ```

    **c**: **Sign with**
    ```
    CERTAUTH
    ```

    **d**: **Sign Label**
    ```
    TESTCA
    ```

    **e**: **Common Name**
    ```
    company.com
    ```

    **f**: **Expiration Date**
    
    Enter a date that falls within the next 30 days in the format YYYY-MM-DD.

    !!! Note "The following image does not highlight all the fields that need to be modified!"

    ![](_attachments/cert-18a.png) 

3. Click **Launch**.

    ![](_attachments/cert-19a.png) 

4. Verify that the **DEMOCERT** was successfully created.

    ![](_attachments/cert-20a.png) 

## Import the Ansible automations into watsonx Orchestrate

For this use case, configure the assistant to guide the user through the process of identifying their SSL certificate’s expiration date and automate the certificate renewal process. To do so, import the needed AAP templates into watsonx Orchestrate as skills.

For this use case, the ansible templates you import are:

- z/OS Certs – List Cert
- z/OS Certs – Search and Renew
- Retrieve job output (utility skill)

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
    certs
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

    ![](_attachments/c-skill-4a.png)

5. Expand the **Ansible Job Template Proj...** folder and then click **aap4zos**.

    ![](_attachments/c-skill-5a.png)

6. Select **Z/os certs - list cert** and **Z/os certs - search and renew** and then click **Save as draft**.

    !!! Tip "Scroll through the table of skills to find the required skills."

    ![](_attachments/c-skill-6a.png)

7. Click the ellipses (![](../_attachments/ellipsesIcon.png)) for the **z/OS Certs - List Cert** skill and select **Enhance this skill**.

    ![](_attachments/c-skill-7a.png)

8.  Review the skill enhancement options and then click **Publish**.

    ![](_attachments/c-skill-8a.png)

9.  Repeat steps 7 and 8 for the **z/OS Certs - Search and Renew** skill.

10. **Challenge**: You also need to add the **Retrieve job output** utility to your **certs** app just like you did when creating the **Gather Facts** skill flow. Repeat steps 2 - 8 to add the **Retrieve job output** utility skill to your **certs** app. 

## Verify all the skills are successfully imported and create the app connection.

1. Open **Skill catalog** in watsonx Orchestrate.

    ![](_attachments/c-skill-9a.png)

2. Enter **certs** in the search bar.

    ![](_attachments/c-skill-10a.png)

3. Click the **certs** tile.

    ![](_attachments/c-skill-11a.png)

4. Click **Add skill +** for each of the 3 skills in the certs app.

    ![](_attachments/c-skill-12a.png)

5. Click **Connect app**.

    ![](_attachments/c-skill-13a.png)

6. Enter your **AAP Username** and **AAP Password** and then click **Connect app**.

    ![](_attachments/c-skill-14a.png)

7. Verify that the app is connected.

    ![](_attachments/c-skill-15a.png)

## Connect the app to the assistant.

1. Open **Skill catalog** in watsonx Orchestrate.

    ![](_attachments/c-app-1.png)

2. Select the **Draft** version of your assistant and click **Connections**.

    ![](_attachments/c-app-2.png)

3. Enter **certs** in the search bar.

    ![](_attachments/c-app-3.png)

4. Click the ellipses (![](../_attachments/ellipsesIcon.png)) for the *certs** app and select **Connect app**.

    ![](_attachments/c-app-4.png)

5. Click **Connect app**.

    ![](_attachments/c-app-5.png)

6. Enter your **AAP Username** and **AAP Password** and then click **Connect app**.

    ![](_attachments/c-app-6.png)

## Create a skill flow to retrieve certificate expiration dates.
The goal of this scenario is to configure the assistant to automate the certificate renewal process for the client. The first step in that process is to help the SA identify the expiration date of their z/OS certificate. You have imported the **z/OS Certs – List Cert** skill from Ansible Automation Platform. Next, create a new skill flow that uses the **z/OS Certs – List Cert** skill that can later be used in a natural conversation through assistant actions.

First, create a skill flow to retrieve and display the expiration date of a z/OS certificate based on the certificate label the user provides.

1. Open **Skill studio** in watsonx Orchestrate.

    ![](_attachments/c-sf-1a.png) 

2. Click **Create** and then click **Skill flow**.

    ![](_attachments/c-sf-2a.png) 

3. Click the **+** icon.

    ![](_attachments/c-sf-3a.png) 

4. Click the **certs** app.

    !!! Tip "Search on **certs** if you do not see the tile for your app."

    ![](_attachments/c-sf-4a.png)

5. Click **Add Skill +** in the **z/OS Certs - List Cert** tile.

    ![](_attachments/c-sf-5a.png)

6. Click the **+** icon **below** the **z/OS Certs - List Cert** skill and repeat steps 4 and 5 to add the **Retrieve job output** skill.

7. Click the **Retrieve job output** skill.

    ![](_attachments/c-sf-6a.png)

8. Click the **id** field.

    ![](_attachments/c-sf-7a.png)

9. Click **z/OS certs - List Cert**.

    ![](_attachments/c-sf-9a.png)

10. Click **job**.

    ![](_attachments/c-sf-10a.png)

11. Click the **z/OS Certs - List Cert** tile.

    ![](_attachments/c-sf-11a.png)

12. On both the **Input** and **Output** tabs for the **z/OS Certs - List Cert** skill, enable the **Hide this form from the user** options.

    To enhance the user experience, hide the input and output forms from the user. This disables the List Cert skill form from being displayed. Rather than the user entering in their certificate details as input to the skill form, those details can be gathered into the skill through user prompts when creating an assistant action. This enables a more natural conversation flow when interacting with the assistant.

    ![](_attachments/c-sf-12a.png)

13. Repeat step 12 for the **Output** of the **Retrieve job output** skill.

    ![](_attachments/c-sf-13a.png)

The output of the **List Cert** skill includes a large amount of data. In the assistant, only the **certificate expiration date** is needed. In the next steps, transform the output to return only the **certificate expiration date**.

14. Click the **+** icon **below** the **Retrieve job output** skill.

    ![](_attachments/c-sf-14a.png)

15. Click the **Custom forms**.

    ![](_attachments/c-sf-15a.png)

16. Click **Add skill +** in the **Input form**.

    ![](_attachments/c-sf-16a.png)

17. Click the **Input form** skill.

    ![](_attachments/c-sf-17a.png)

18. Click **Add input field +**.

    ![](_attachments/c-sf-18a.png)

19. Click **Paragraph text** and then click **Next**.

    ![](_attachments/c-sf-19a.png)

20. Enter `certificate expiration date` in the **Display label** field and click **Apply**.

    **Display label**:
    ```
    certificate expiration date
    ```

    ![](_attachments/c-sf-20a.png)

21. Click the **certificate expiration date** entry field.

    ![](_attachments/c-sf-21a.png)

22. In the **Mapping data for "certificate expiration data"** section, click **Retrieve job output**.

    ![](_attachments/c-sf-22a.png)

23. Click **content**.

    ![](_attachments/c-sf-23a.png)

24. Click **Add transformation +**.

    A **transformation** is used to extract the **certificate expiration date** from all the job output data.

    ![](_attachments/c-sf-24a.png)

25. Click the **Select** drop-down and select **Get substring by regular expression**.

    ![](_attachments/c-sf-25a.png)

26. Cut and paste the *regular express* that follows to extract the certificate end date and then click **Add**.

    **Regular Expression**:
    ```
    (?<=End Date: ).[^"]*
    ```

    !!! Note 

        There are several ways to transform data to match the type of output you need. In the above example, regular expression is used to get the needed output (the certificate expiration date). This regular expression was tested against the output of the z/OS Certs – List Cert Ansible job to extract the value assigned to the ‘End Date’ field in the job’s output. After completing this use case, you can experiment with other regular expressions to extract additional information from the job’s output. 

        For more information on transforming data within watsonx Orchestrate, review the documentation found <a href="https://www.ibm.com/docs/en/watsonx/watson-orchestrate/current?topic=flows-mapping-values-input-fields#transforming-data" target="_blank">here</a>.

    ![](_attachments/c-sf-26a.png)

27. Enter `certificate expiration date` in the form title and toggle the **Hide this form from the user** option.

    **Form title**:
    ```
    certificate expiration date
    ```

    ![](_attachments/c-sf-27a.png)

Next, create an output for to return the transformed data from the input skill.

28.  Click the **+** icon **below** the **Input form**.
    
    ![](_attachments/c-sf-28a.png)

29.  Click **Custom forms**.

    ![](_attachments/c-sf-29a.png)

30. Click **Add Skill +** in the **Output form** tile.

    ![](_attachments/c-sf-30a.png)

31. Click the **Output form** skill.

    ![](_attachments/c-sf-31a.png)

32. Click in the **Custom forms** field and enter `#` (the pound key, also know as the number sign or hash key).

    Typing the `#` opens a new dialog window.

    ![](_attachments/c-sf-32a.png)

33. Expand (**a**) **Input form**, select (**b**) **certificate expiration date**, and then click (**c**) **OK**.

    ![](_attachments/c-sf-33a.png)

34. Enable the **Hide this form from the user** option.

    !!! Question "Why hide some of the forms?"

        You may be wondering why hide the input and output forms for the skills in the skill flow. This is done to execute the automation based on user prompts for the inputs of the skills. This is done through natural conversation with the assistant when the skill flow is configured as an assistant ‘action’ (you will do this soon). Although the final output is hidden, it is accessible as a variable in a custom-built action. The value can be displayed exactly at the point it is expected in the conversation.

    ![](_attachments/c-sf-34a.png)

35. Click the **pencil** icon (![](../../skills/_attachments/pencilIcon.png)).

    ![](_attachments/c-sf-35a.png)

36. Enter `Retrieve certificate expiration` in the **Name** field and click **Save**.

    **Name**:
    ```
    Retrieve certificate expiration
    ```

    ![](_attachments/c-sf-36a.png)

37. Click **Actions** and then click **Save as draft**.

    ![](_attachments/c-sf-37a.png)

38. Click **Actions** and then click **Enhance**.

    ![](_attachments/c-sf-38a.png)

39. Review the skill flow settings and click **Publish**.

    ![](_attachments/c-sf-39a.png)

You created a new skill flow that accomplishes part of the use case – retrieving and displaying the expiration date of a z/OS certificate based on the certificate label the user provides.

In the next section, you will create a simpler skill flow for the z/OS Certs – Search and Renew skill that you previously imported. After this additional skill flow is created, add both skill flows as skill-based actions to be called in a custom-built action that maps inputs to the skill flows through natural conversation.

## Create a skill flow for certificate renewal
The final step before configuring the assistant with actions is to create a skill flow for renewing certificates. Recall the z/OS Certs – Search and Renew automation imported the from Ansible Automation Platform earlier. The skill flow that you create next is composed of that single skill. There is no need to return the output. After the automation is triggered, the user can verify the new expiration date by running the retrieve certificate expiration date flow.

1. In **Skill studio**, click **Create** and then click **Skill flow**.

    ![](_attachments/c-sf2-1a.png)

2. Click the **+** icon.

    ![](_attachments/c-sf-3a.png) 

3. Click the **certs** app.

    !!! Tip "Search on **certs** if you do not see the tile for your app."

    ![](_attachments/c-sf-4a.png)

4. Click **Add Skill +** in the **z/OS Certs - Search and Renew** tile.

    ![](_attachments/c-sf2-4a.png)

As mentioned, there is no need to return the Ansible job output of this skill when it is run. The **z/OS Certs - Search and Renew** is used to set default values for some of the inputs. In this use case, assume that the SA will be renewing their SITE certificates that are signed with a previously generated certificate authority.

5. Click the **z/OS Certs - Search and Renew** skill.

    ![](_attachments/c-sf2-5a.png)

6. Click **Input**.

    ![](_attachments/c-sf2-6a.png)

7. Hover over the **extra_vars.cert_type_survey** input field and click the pencil icon (![](../_attachments/pencilIcon.png)).

    ![](_attachments/c-sf2-7a.png)

8. Click in the **extra_vars.cert_type_survey** input field and enter `SITE`.

    **extra_vars.cert_type_survey**:
    ```
    SITE
    ```

    !!! Warning "Do not enter spaces before or after the word `SITE`."

    ![](_attachments/c-sf2-8a.png)

9. Repeat 7 and 8 for the **extra_vars.sign_with_survey** field and enter the word `CERTAUTH`.

    **extra_vars.sign_with_survey**:
    ```
    CERTAUTH
    ```

    !!! Warning "Do not enter spaces before or after the word `CERTAUTH`."

    ![](_attachments/c-sf2-9a.png)

10. Enable the **Hide this form from the user** option for both the **Input** and **Output** forms.

    !!! Note "The image that follows only shows the **Output** form, but enable the option for both forms."

    ![](_attachments/c-sf2-10a.png)

11. Click (**a**) the pencil icon (![](../_attachments/pencilIcon.png)) for the skill flow, enter (**b**) `Cert Renewal skill flow` in the **Name** field, and click (**c**) **Save**.

    **Name**: 
    ```
    Cert Renewal skill flow
    ```

    ![](_attachments/c-sf2-11a.png)

12. Click **Actions** and then click **Save as draft**.

    ![](_attachments/c-sf2-12a.png)

13. Click **Actions** and then click **Enhance**.

    ![](_attachments/c-sf2-13a.png)

14. Review the skill flow settings and click **Publish**.

    ![](_attachments/c-sf2-14a.png)

## Add the skill flows to the assistant
Next, create 2 skill-based actions that use the skill flows. The skill-based actions enable the ability to call the skill flow as a subaction within a new custom-built action. For this use case, create two skill-based actions that use the previously created skill flows:

- Retrieve certificate expiration – maps the user prompted certificate label as input and extracts the certificate expiration date from the Ansible job’s output.

- Cert Renewal skill flow – maps the user prompted certificate label and new expiration date as input and runs the Search and Renew Ansible job to extend the expiration date of the certificate.
  
After the 2 skill flows are added as skill-based actions, integrate the actions into a custom-built action that defines the entire conversation flow. The flow assists the SA with the certificate renewal process.

1. Open **AI assistant builder** in watsonx Orchestrate.

    ![](_attachments/c-actions-1a.png)

2. Click **Actions**.

    ![](_attachments/c-actions-2a.png)

3. Click **New action+**.

    ![](_attachments/c-actions-3a.png)

4. Click **Skill-based action**.

    ![](_attachments/c-actions-4a.png)

5. Click the **Retrieve certificate expiration** tile and then click **Next**.

    ![](_attachments/c-actions-5a.png)

6. Click **Cancel** on the **New action** dialog.

    !!! Note "For this use case, the action is triggered from a custom-built action. To prevent the skill flow from being run as the skill-based action, do not enter any example phrases."

    ![](_attachments/c-actions-6a.png)

7. Click **x** to close the **Retrieve certificate expiration** skill.

    ![](_attachments/c-actions-7a.png)

8. Repeat steps 3 - 7 to create a skill-based action for the **Cert Renewal skill flow**.

    !!! Note "This action is also triggered from a custom-built action. Do not enter any example phrases."

9. Verify that both skill-based actions are available.

    ![](_attachments/c-actions-9a.png)

## Create a custom-built action for SSL Certificate Renewal
Next, create a custom-built action that runs the new skill-based actions as subactions. Configure the custom-built action to enable a natural conversation with the assistant, gather relevant details from the user, and map those details to the action inputs. 

1. Click **New action +**.

    ![](_attachments/c-custom-1a.png)

2. Click **Custom-built-action**.

    ![](_attachments/c-custom-2a.png)

3. Enter `z/OS certificate expires soon` and then click **Save**.

    **What does your customer say to start this interaction**:
    ```
    z/OS certificate expires soon
    ```

    ![](_attachments/c-custom-3a.png)

The conversational search capability that is provided by watsonx Assistant for Z can provide step-by-step guidance for determining certificate expiration and renewing certificates, and is grounded on Z domain-specific knowledge. In the first step to be taken when the user prompts the assistant with `z/OS certificate expires soon`, configure the assistant to use conversational search to provide a response on the process and the ability to automate the process.

4. Click the **And then** drop down and select **Search for the answer**.

    ![](_attachments/c-custom-4a.png)

The result is that anytime the user input matches the example phrase `z/OS certificate expires soon`, the first step that is taken is for the assistant to use conversational search and provide a response to their original question.

Like in the IPL Information scenario, add a custom search query so when conversational search is run in the first conversation step, the query used is hardcoded and not what the user input.

5. Click **Edit settings**.

    ![](_attachments/c-custom-5a.png)

6. Enter the following prompt to be used in the **Custom search query** field and then click **Apply**.

    **Custom search query**:
    ```
    My z/OS certificate is going to expire soon. How do I retrieve the expiration date for my certificate?
    ```

    ![](_attachments/c-custom-6a.png)

7. Click **Next step+**.

    ![](_attachments/c-custom-7a.png)

8. Enter the following response in the **Assistant says** field.

    **Assistant says**:
    ```
    Would you like to run the skill to retrieve your certificate’s expiration date?
    ```

    ![](_attachments/c-custom-8a.png)

9. Click the **Define customer response** option list and select **Confirmation**.

    The **Confirmation** option prompts the user to select `Yes` or `No`.

    ![](_attachments/c-custom-9a.png)

10. Click **Next step +**.

    ![](_attachments/c-custom-10a.png)

11. Click the **Is taken** option list and select **with conditions**.

    This step handles the flow when the user selects `Yes` in the previous step, indicating that they want to run the skill to retrieve the certificate’s expiration date. To run the **Retrieve certificate expiration action** created earlier, the assistant  needs the certificate label. This label is mapped as input to the skill.

    ![](_attachments/c-custom-11a.png)

12. Enter the following text in the **Assistant says** field.

    **Assistant says**:
    ```
    What is your certificate label?
    ```

    ![](_attachments/c-custom-12a.png)

13. Click the **Define customer response** drop-down list and select **Free text**.

    ![](_attachments/c-custom-13a.png)

14. Click **Next step+**.

    ![](_attachments/c-custom-14a.png)

15. Click the **Is taken** option list and select **with conditions**.

    After the user enters the certificate label as free text, the next step is to run the **Retrieve certificate expiration** skill-based action created earlier. To do so, map the user input to the skill flow and retrieve the expiration date for that certificate.

    ![](_attachments/c-custom-15a.png)

16. Click the **And then** option list and click **Go to a subaction**.

    Notice that the default condition validates the *free text* is defined from the previous step.

    ![](_attachments/c-custom-16a.png)

17. Click the (**a**) **Go to** option list, select the (**b**) **Retrieve certificate expiration** skill-based action, and then click (**c**) **Apply**.

    ![](_attachments/c-custom-17a.png)

18. Click **Edit passed values**.

    To run the **Retrieve certificate expiration** subaction that uses the users certificate label, the passed value needs to be modified.

    ![](_attachments/c-custom-18a.png)

19. Click **Set new value +** and then select **extra_vars.cert_label_survey**.

    ![](_attachments/c-custom-19a.png)

20. In the **To** field, select **Action step variables**, and then select **What is your certificate label?**.

    ![](_attachments/c-custom-20a.gif)

21. Click **Apply**.

    ![](_attachments/c-custom-21a.png)

22. Click **Next step +**.

    ![](_attachments/c-custom-22a.png)

23. Click the **Is taken** option list and select **with conditions**.

    In the previous step, you configured the assistant to run the Retrieve certificate expiration subaction you created, passing the certificate label the user inputted to the skills inputs. Recall when the **Retrieve certificate expiration** skill flow was created, the output form at the end of the skill flow was hidden. That form contained the expiration date. As a result, nothing is returned when running the subaction in the previous step. Now, configure the custom-action to provide that output as a response.

    ![](_attachments/c-custom-23a.png)

24. Enter the following text in the **Assistant says** field.

    **Assistant says**:
    ```
    Below is your certificate’s expiration date:
    ```

    ![](_attachments/c-custom-24a.png)

25. While still in the **Assistant says** field, press **return** and then type `$`.

    !!! Note "The `$` is a special key that lists available functions. The following image is edited to show that you must type the `$`, but it is not displayed on your screen."

     ![](_attachments/c-custom-25a.png)
    
26. Click **Retrieve certificate expiration (step 4)** and then click **Retrieve certificate expiration result variable**.

     ![](_attachments/c-custom-26a.gif)

27. Review the **Assistant says** field and then click **Save** (![](../_attachments/diskIcon.png)).

     ![](_attachments/c-custom-27a.png)

## Test the **z/OS certificate expires soon** custom-built skill
Before completing the use case, test the **z/OS certificate expires soon** custom-built skill that uses the **DEMOCERT** certificate created earlier.

1. Click **Preview**.

    ![](_attachments/a-preview1-1a.png)

2. Enter the following prompt in the preview.

    **Prompt**:
    ```
    My z/OS certificate is going to expire soon. How do I retrieve the expiration date for my certificate?
    ```
    ![](_attachments/a-preview1-2a.png)

3. Review the response and click **Yes**.

    The assistant responds by calling Conversational search and returns a response by using the Z RAG, displaying the RACDCERT command that can be used. The assistant then prompts `Would you like to run the skill to retrieve?`.

    ![](_attachments/a-preview1-3a.png)

4. Review the response and enter `DEMOCRT`.

    **Prompt**:
    ```
    DEMOCRT
    ```

    ![](_attachments/a-preview1-4a.png)

5. Click **Apply**.

    ![](_attachments/a-preview1-5a.png)

6. Review the response.

    If you see the following response (the date may differ), the custom-built skill ran successfully. The output of the skill flow was not the entire output of the z/OS Certs – List Cert Ansible job, but rather the certificate expiration date that was extracted from the full job output by using the Regular Expression transformation. 

## Complete the custom-built skill to renew the certificate
Now that the custom-built action is working, add steps to include the certificate renewal process. After retrieving and displaying the user’s certificate expiration date, ask the user if they want to renew the certificate, and if so, prompt for the new date and renew the certificate.

1. Click **Next step +**.

    ![](_attachments/addLastSteps-1a.png)

2. Click the **Is taken** option list and select **with conditions**.

    ![](_attachments/addLastSteps-2a.png)

3. Enter the following text in the **Assistant says** field.

    **Assistant says**:
    ```
    Would you like to renew your certificate?
    ```

    ![](_attachments/addLastSteps-3a.png)

4. Click the **Define customer response** option list and select **Confirmation**.

    ![](_attachments/addLastSteps-4a.png)

5. Click **Next step +**.

    ![](_attachments/addLastSteps-5a.png)

6. Click the **Is taken** option list and select **with conditions**.

    This step handles the flow in which the user selects `Yes` in the previous step indicating they want to renew their expiring certificate. Before initiating the Cert Renewal skill flow action to automate this, the assistant first needs the new expiration date for the certificate.

    ![](_attachments/addLastSteps-6a.png) 

7. Enter the following text in the **Assistant says** field.

    **Assistant says**: 
    ```
    What date would you like to set the renewed certificate’s expiration date to? Please enter in the form of YYYY-MM-DD.
    ```

    ![](_attachments/addLastSteps-7a.png)

8. Click the **Define customer response** option list and select **Free text**.

    ![](_attachments/addLastSteps-8a.png)

9. Click **New step +**.

    ![](_attachments/addLastSteps-9a.png)

10. Click the **Is taken** option list and select **with conditions**.

    With the new expiration date entered by the user, the next step is to run the Cert Renewal skill flow action as a subaction. Next, trigger the renewal skill flow and pass the user provided details as input to the action to renew the certificate and extend the certificates expiration date.

    ![](_attachments/addLastSteps-10a.png)

11. Enter the following text in the **Assistant says** field.

    This assistant first responds with the message that follows before triggering the certificate renewal skill-flow. When performing a demo of this use case, mention the **z/OS Certs – Search and Renew** Ansible playbook typically takes a minute or so to complete. 

    **Assistant says**: 
    ```
    Renewing your certificate…this could take up to a minute. Please wait one minute before selecting an option below.
    ```

    ![](_attachments/addLastSteps-11a.png)

12. Click the **And then** option list and select **Go to a subaction**.

    ![](_attachments/addLastSteps-12a.png)

13. Click the **Go to** option list and select the **Cert Renewal skill flow**.

    ![](_attachments/addLastSteps-13a.png)

14. Click **Apply**.

    ![](_attachments/addLastSteps-14a.png)

15. Click **Edit passed values**.

    Edit the passed values to use them in the **Cert Renewal** skill flow subaction.

    ![](_attachments/addLastSteps-15a.png)

16. Click **Set new value +** and then select **extra_vars.cert_label_survey**.

    ![](_attachments/addLastSteps-16a.png)

17. In the **To** field, select **Action step variables**.

    ![](_attachments/addLastSteps-17a.png)

18. Click **What is your certificate label?**

    ![](_attachments/addLastSteps-18a.png)

19. Repeat steps 16 - 18 adding the **extra_vars.new_expiry-date_survey** input variable and **What date would you like to set the...** in the **To** field.

    ![](_attachments/addLastSteps-19a.png)

20. Click **Set new value +** and then select **extra_vars.sign_label_survey**.

    ![](_attachments/addLastSteps-20a.png)

21. In the **To** option list, select **Enter text**.

    ![](_attachments/addLastSteps-21a.png)

22. Enter `TESTCA` in the **Enter text** field and click **Apply** for the **To** option list.

    **Enter text**:
    ```
    TESTCA
    ```

    For this passed value, hardcode `TESTCA` in the skill flow’s input for the sign_label variable. This is the CA certificate created earlier for demo purposes in the AAP web console.

    ![](_attachments/addLastSteps-22a.png)

23. Review the **Edit passed values** and then click **Apply**.

    Review all 3 variables are set correctly.

    ![](_attachments/addLastSteps-23a.png)

24. Click **Next step +**

    ![](_attachments/addLastSteps-24a.png)

25. Click the **Is taken** option list and select **with conditions**.

    To complete the flow, ask the user if they want to verify the new expiration date.

    ![](_attachments/addLastSteps-25a.png)

26. Enter the text that follows in the **Assistant says** field.

    **Assistant says**:
    ```
    Would you like to verify the new expiration date for your certificate?
    ```

    ![](_attachments/addLastSteps-26a.png)

27. Click the **Define customer response** option list and select **Confirmation**.

    ![](_attachments/addLastSteps-27a.png)

28. Click **Next step +**.

    ![](_attachments/addLastSteps-28a.png)

29. Click the **Is taken** option list and select **with conditions**.

    On the condition that the user selected `Yes` in the previous step, configure a step to run the **Retrieve certificate expiration** skill-flow again to retrieve and display the new expiration date of the renewed certificate.

    ![](_attachments/addLastSteps-29a.png)

30. Click the **And then** option list and select **Go to a subaction**.

    ![](_attachments/addLastSteps-30a.png)

31. Click the (**a**) **Go to** option list, select (**b**) **Retrieve certificate expiration**, and then click (**c**) **Apply**.

    ![](_attachments/addLastSteps-31a.png)

32. Click **Edit passed values**.

    ![](_attachments/addLastSteps-32a.png)

33. Click **Set new value +** and then select **extra_vars.cert_label_survey**.

    ![](_attachments/addLastSteps-33a.png)

34. In the **To** option list, click **Action step variables**.

    ![](_attachments/addLastSteps-34a.png)

35. Click **What is your certificate label?**.

    ![](_attachments/addLastSteps-35a.png)

36. Review the **Edit variable values** and click **Apply**.

    ![](_attachments/addLastSteps-36a.png)

37. Click **Next step +**.

    ![](_attachments/addLastSteps-37a.png)

38. Click the **Is taken** option list and select **with conditions**.

    The final step is to display new expiration date of the certificate. Nothing is returned in the previous step when running the Retrieve certificate expiration skill flow - this was because the output form was hidden when the skill was created. In this step, provide the output as an assistant response to the user, with only the expiration date extracted from the full job output.

    ![](_attachments/addLastSteps-38a.png)

39. Enter the following text in the **Assistant says** field.

    **Assistant says**:
    ```
    Below is the new expiration date of your renewed certificate:
    ```

    ![](_attachments/addLastSteps-39a.png)

40. While still in the **Assistant says** field, press **return** and then type `$`.

    !!! Note "The `$` is a special key that lists available functions. The following image is edited to show that you must type the `$`, but it is not displayed on your screen."

    ![](_attachments/addLastSteps-40a.png)

41. Click **Retrieve certificate expiration (step 10)**.

    !!! Warning "Be sure to select the output from **step 10** and not step 4."

    ![](_attachments/addLastSteps-41a.png)

42. Click **Retrieve certificate expiration result variable**.

    ![](_attachments/addLastSteps-42a.png)

43. Click the **And then** option list and select **End the action**.

    ![](_attachments/addLastSteps-43a.png)

44. Review the (**a**) final step, click (**b**) **Save** (![](../_attachments/diskIcon.png)), and then click (**c**) **x**.

    ![](_attachments/addLastSteps-44a.png)

## Run the complete custom-built action
The custom-built action is now complete and can be demonstrated to the SA for this use case. In demonstrating the ability to infuse Ansible automations into a natural conversation, the SA is able to see the value that watsonx Assistant for Z can provide in helping them improve productivity and remove the need to go to their senior colleagues for assistance.

1. Open **Preview** in the **Ai assistant builder**.

    ![](_attachments/aa-prview2-1a.png)

2. Enter the following text in the assistant.

    **Prompt**:
    ```
    How do I check the expiration date for my certificate that’s expiring soon?
    ```

    !!! Tip "Use the **Change layout** option to open a full page view of the assistant."

    ![](_attachments/aa-prview2-2a.png)    

3. Review the response and click **Yes**.

    The assistant responds with conversational search, providing a content-grounded answer based on IBM Z documentation. The response includes a RACF command that the SA might use to determine their certificate’s expiration date.

    Following the response, the assistant prompts the user if they want to run the skill to retrieve a certificate’s expiration date.

    ![](_attachments/aa-prview2-3a.png)

4. Enter `DEMOCERT` after the assistant responds with **What is your certificate label?**

    **Prompt**:
    ```
    DEMOCERT
    ```

    ![](_attachments/aa-prview2-4a.png)

5. Wait 10 seconds and then click **Apply**.

    ![](_attachments/aa-prview2-5a.png)    

6. Review the response and then click **Yes**.

    By providing the automation within the assistant conversation, it makes it very quick for the SA to identify the certificate’s expiration date. In addition to providing this valuable information, the assistant is configured with another automation to renew the certificate if they choose to do so.

    !!! Warning "The expiration date you see may differ from the image that follows."

    ![](_attachments/aa-prview2-6a.png)

7. Enter a date in the future in the format **YYYY-MM-DD**.

    ![](_attachments/aa-prview2-7a.png)

8. Review the response, wait 30 seconds to a minute, and then click **Yes**.

    !!! Warning "It is crucial that you wait 30 seconds to a minute before selecting Yes."
    
        This is because in the background, your z/OS Certs – Search and Renew automation is running within AAP (which you can verify within the AAP Web console). This is mapping the user-inputted expiration date as well as the original certificate label provided by the end-user to the inputs of this AAP automation."

    ![](_attachments/aa-prview2-8a.png)

9. Wait 10 seconds and then click **Apply**.

    ![](_attachments/aa-prview2-9a.png)

10. Review the response.

    The response should match the date that is entered in step 7.

    ![](_attachments/aa-prview2-10a.png)    

In the demo, the SA receives immediate guidance on identifying the certificate expiration date via RACF commands. The SA runs automation that is proposed by the assistant to retrieve the certificate information. Also, because the assistant is configured with step-by-step conversation flows, it is possible to add other prompts within the conversation. For example, proposing the automation of renewing the certificate on their behalf. By doing so, the SA is able to reduce the time it takes to complete this routine task.

Recall how many steps were involved in the Ansible template for **z/OS Certs – Search and Renew**. By automating these tasks with Ansible, the System Administrator streamlines the entire process and ensures that their critical certificates are up to date and reduce the risk of expired certificates disrupting their business services.
