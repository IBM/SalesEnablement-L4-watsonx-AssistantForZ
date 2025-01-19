# Use case: SSL Certificate renewal on z/OS
Now, shift roles to that of a mainframe Security Administrator. Your client would like to understand how watsonx Assistant for Z can help them to ensure their critical security certificates are up-to-date and reduce the risk of expired certificates disrupting their organization’s services. 

Secure Sockets Layer (SSL) certificates, often referred to as digital certificates, are used to establish an encrypted connection between communicating parties over a network. Certificate management is crucial for maintaining the security of a company’s z/OS environment. It has been a while since the Security Administrator performed the  tasks to manage and renew a certificate, but recalls there being many steps required on z/OS and various RACF commands that need run to renew a certificate. Rather than going to their senior Security Administrator for assistance, you can demonstrate how leveraging watsonx Assistant for Z can help them automate the certificate renewal process by infusing Ansible automation into natural conversation, as well as providing step-by-step guidance on the processes to determine certificate expiration and renewing certificates.

In this scenario, you will leverage the Ansible automation templates provided with your AAP and WAZI z/OS environment to create assistant actions to guide the client through the process of identifying their SSL certificate’s expiration dates, and automating the certificate renewal process for them. This will save them time and improve their productivity.

## Create an initial Certificate Authority (CA) certificate to sign future SITE certificates
For this use case, a Certificate Authority (CA) certificate is needed to sign new SITE certificates. 

1. Open and log into the Ansible Automation Platform (AAP) web console. 

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

     In the output of the playbook, notice a new keyring was created, a certificate was created, and the certificate ws connected to the key ring.

     ![](_attachments/cert-5a.png)    

7. Locate the line **TASK [GENERATE new certificate]**, click the **changed: [zos host]**.

     ![](_attachments/cert-6a.png)   

8.  Click **JSON**.

     ![](_attachments/cert-7a.png)  

9.  Review the **RACDCERT** command that was run to generate the certificate and then click **x** to close the window.

     ![](_attachments/cert-8a.png)  
    
## Create an *expiring* certificate
Now create an expiring certificate using the CA certificate you just created.

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

    Unlike the first certificate you created which was *self-signed*, this certificate will be signed by the local certificate authority using the CA you created.

    !!! Note "The image below does not highlight all the fields that need to be modified!"

    ![](_attachments/cert-10a.png)  

3. Click **Launch**.

     ![](_attachments/cert-11a.png)  

4. Verify the job was successful and inspect the output of the job.

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

    !!! Note "The image below does not highlight all the fields that need to be modified!"

    ![](_attachments/cert-14a.png)  

3. Click **Launch**.

    ![](_attachments/cert-15a.png)  

4. Verify the job was **Successful** and review the output.

    **Note**: you may need to click the **Reload Output** button after the job completes to view the full output.

    Review the tasks that were run within the automation to renew the certificate. Some of the steps completed include:

    - Run the RACF_CERTIFICATE_EXPIRATION z/OS Health Check

    - Submit JCL to pull a report on the z/OS Health Check

    - Search the output of the report for the given certificate label

    - Print out the expiring certificate, if it is found. You should see: ‘TESTSITE expiring – True’

    - If the certificate is expiring, kick off a series of RACDCERT commands to do the following:
  
         - Backup the expiring certificate
       
         - Rekey the certificate and give it a new temporary label
       
         - Generate a CSR for the new certificate
       
         - Sign the new certificate with the local CA
       
         - Delete the old certificate
       
         - Relabel the new certificate to use the same label as before
       
         - Refresh the digital certificate list
  
     ![](_attachments/cert-16a.png) 

## Create another *expiring* certificate
Create one more expiring certificate to use with the your assistant and new skills you will create.

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

    !!! Note "The image below does not highlight all the fields that need to be modified!"

    ![](_attachments/cert-18a.png) 

3. Click **Launch**.

    ![](_attachments/cert-19a.png) 

4. Verify the **DEMOCERT** was successfully created.

    ![](_attachments/cert-20a.png) 

## Import the Ansible automations into watsonx Orchestrate

For this use case, configure the assistant to guide the user through the process of identifying their SSL certificate’s expiration date and automate the certificate renewal process. To do so, import the needed AAP templates into watsonx Orchestrate as skills.

For this use case, the ansible templates you will import are:

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

    Use the **URL**, **User Name**, and **Password** values recorded in the [Explore Ansible Automation Platform](exploreAAP.md) section earlier.

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

7. Verify the app is connected.

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
The goal of this scenario to configure the assistant to automate the certificate renewal process for the client. The first step in that process is to help the security administrator identify the expiration date of their z/OS certificate. You have now imported the **z/OS Certs – List Cert** skill from Ansible Automation Platform. Next, create a skill flow using that skill which can later be leveraged in a natural conversation through assistant actions.

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

    To enhance the user experience, hide the input and output forms from the user. This will disable the List Cert skill form from being displayed. Rather than the user entering in their certificate details as input to the skill form, those details can be gathered into the skill through user prompts when creating an assistant action. This enables a more natural conversation flow when interacting with the assistant.

    ![](_attachments/c-sf-12a.png)

13. Repeat step 12 for the **Output** of the **Retrieve job output** skill.

    ![](_attachments/c-sf-13a.png)

The output of the **List Cert** skill includes a large amount of data. In the assistant, only the **certificate expiration date** is needed. In the next steps, transform the output to only return the **certificate expiration date**.

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

27. Enter `certificate expiration date` in the form title and toggle on the **Hide this form from the user** option.

    **Form title**:
    ```
    certificate expiration date
    ```

    ![](_attachments/c-sf-27a.png)

Next, create an output for to return the the transformed data from the input skill.

28.  Click the **+** icon **below** the **Input form**.
    
    ![](_attachments/c-sf-28a.png)

29.  Click **Custom forms**.

    ![](_attachments/c-sf-29a.png)

30. Click **Add Skill +** in the **Output form** tile.

    ![](_attachments/c-sf-30a.png)

31. Click the **Output form** skill.

    ![](_attachments/c-sf-31a.png)

32. Click in the **Custom forms** text entry field and enter `#` (the pound key, also know as the number sign or hash key).

    Typing the `#` will open a new dialog window.

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

Congratulations! You’ve just created a new skill flow that accomplishes part of the use case – retrieving and displaying the expiration date of a z/OS certificate based on the certificate label the end-user provides. You will see this in action in the following sections.

In the next section, you will create one more simpler skill flow for the z/OS Certs – Search and Renew skill you previously imported. Once this additional skill flow is created, you will add both skill flows as skill-based actions which can be called in a custom-built action to map inputs to the skill flows through natural conversation.

Page 164
    ![](_attachments/c-sf2-1a.png)
