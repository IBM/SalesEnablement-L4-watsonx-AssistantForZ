# Use case: SSL Certificate renewal on z/OS
Now, shift roles to that of a mainframe Security Administrator. Your client would like to understand how watsonx Assistant for Z can help them to ensure their critical security certificates are up-to-date and reduce the risk of expired certificates disrupting their organization’s services. 

Secure Sockets Layer (SSL) certificates, often referred to as digital certificates, are used to establish an encrypted connection between communicating parties over a network. Certificate management is crucial for maintaining the security of a company’s z/OS environment. It has been a while since the Security Administrator performed the  tasks to manage and renew a certificate, but recalls there being many steps required on z/OS and various RACF commands that need run to renew a certificate. Rather than going to their senior Security Administrator for assistance, you can demonstrate how leveraging watsonx Assistant for Z can help them automate the certificate renewal process by infusing Ansible automation into natural conversation, as well as providing step-by-step guidance on the processes to determine certificate expiration and renewing certificates.

In this scenario, you will leverage the Ansible automation templates provided with your AAP and WAZI z/OS environment to create assistant actions to guide the client through the process of identifying their SSL certificate’s expiration dates, and automating the certificate renewal process for them. This will save them time and improve their productivity.

## Create an initial Certificate Authority (CA) certificate to sign future SITE certificates
1. Open and log into the Ansible Automation Platform (AAP) web console. 

   !!! tip "Don't remember how to do this?"

      Refer to the first 5 steps in [Explore Ansible Automation Platform](../../skills/exploreAAP.md).

2. Click **Templates** under the **Resources** section.

3. Click the launch icon (![](_attachments/rocketIcon.png)) for the **z/OS Certs - Create Cert** template.

4. On the **Survey** screen, modify the **Certificate Label** and **Type** fields with the values that follow. Leave the default values for all other fields.

    **a**: **Certificate Label**
    ```
    TESTCA
    ```

    **b**: **Type**
    ```
    CERTAUTH
    ```
    
5. Click **Next**.
6. Click **Launch**.
7. Inspect the output of the job.

    In the output of the playbook, notice a new keyring was created, a certificate was created, and the certificate ws connected to the key ring.

8. Below the line **TASK [GENERATE new certificate]**, click the **changed: [zos host]**.
9. Click **JSON**.
10. Review the **RACDCERT** command that was run to generate the certificate.
11. Click **x** to close the window.
    
## Create an *expiring* certificate
1. Return to the **Templates** tab and click the launch icon (![](_attachments/rocketIcon.png)) for the **z/OS Certs - Create Cert** template.
2. On the **Survey** screen, modify the fields that follow with the values specified. Leave the default values for all other fields.

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

    Unlike the first certificate you created which was *self-signed*, this certificate will be signed by the local certificate authority using the CA you created.

3. Click **Next** and then click **Launch**.
4. Inspect the output of the job.

## Renew the *expiring* certificate
1. Return to the **Templates** tab and click the launch icon (![](_attachments/rocketIcon.png)) for the **z/OS Certs - Search and Renew** template.
2. On the **Survey** screen, modify the fields that follow with the values specified. Leave the default values for all other fields.

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
    For now, leave the default date. If you do modify it, in the YYYY-MM-DD format.

3. Click **Next** and **Launch**.
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
  
## Create another *expiring* certificate
1. Return to the **Templates** tab and click the launch icon (![](_attachments/rocketIcon.png)) for the **z/OS Certs - Create Cert** template.
2. On the **Survey** screen, modify the fields that follow with the values specified. Leave the default values for all other fields.

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

3. Click **Next** and then click **Launch**.
4. Verify the **DEMOCERT** was successfully created.

##Import the Ansible automations into watsonx Orchestrate


Page 142