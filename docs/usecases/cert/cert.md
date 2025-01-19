# Use case: SSL Certificate renewal on z/OS
Now, shift roles to that of a mainframe Security Administrator. Your client would like to understand how watsonx Assistant for Z can help them to ensure their critical security certificates are up-to-date and reduce the risk of expired certificates disrupting their organization’s services. 

Secure Sockets Layer (SSL) certificates, often referred to as digital certificates, are used to establish an encrypted connection between communicating parties over a network. Certificate management is crucial for maintaining the security of a company’s z/OS environment. It has been a while since the Security Administrator performed the  tasks to manage and renew a certificate, but recalls there being many steps required on z/OS and various RACF commands that need run to renew a certificate. Rather than going to their senior Security Administrator for assistance, you can demonstrate how leveraging watsonx Assistant for Z can help them automate the certificate renewal process by infusing Ansible automation into natural conversation, as well as providing step-by-step guidance on the processes to determine certificate expiration and renewing certificates.

In this scenario, you will leverage the Ansible automation templates provided with your AAP and WAZI z/OS environment to create assistant actions to guide the client through the process of identifying their SSL certificate’s expiration dates, and automating the certificate renewal process for them. This will save them time and improve their productivity.

## Create an initial Certificate Authority (CA) certificate to sign future SITE certificates
For this use case, a Certificate Authority (CA) certificate is needed to sign new SITE certificates. 

1. Open and log into the Ansible Automation Platform (AAP) web console. 

     !!! tip "Don't remember how?"
    
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
     Enter a date that falls within the next 30 days in the format YYYY-MM-DD.

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

1. 