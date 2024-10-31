# IBM Technology Zone environment
To enable sellers to both learn how to perform client pilots of {{offering.name}}, multiple environments have been created in IBM Technology Zone (ITZ). The environments leveraged for the watsonx Assistant for Z Velocity lab environment can be found in the <a href="{{itz.collectionURL}}" target="_blank">{{itz.collectionName}}</a> collection and consist of:

- **watsonx Assistant for Z lab – watsonx Orchestrate** – a dedicated tenant of watsonx Orchestrate on IBM Cloud and is leveraged for much of the assistant configuration, configuring conversational search, importing skills and configuring assistant actions

- **Ansible Automation Platform (AAP) & z/OS** – a pattern used to deploy a pre-configured instance of AAP and Wazi z/OS with pre-loaded Ansible playbooks that can be imported as skills within watsonx Orchestrate and connected to your assistant. Provides pre-loaded templates for various use cases which will be covered in a later section. To learn more about AAP, go <a href="https://www.redhat.com/en/technologies/management/ansible" target="_blank">here</a>. To learn more about Wazi, go <a href="https://www.ibm.com/cloud/wazi-as-a-service" target="_blank">here</a>.

- **Single Node OpenShift with NFS storage** – used to provision a single-node OpenShift cluster (SNO) on IBM Cloud used to install a dedicated instance of OpenSearch for watsonx Assistant for Z. This environment enables ingestion of client supplied documents.

<!-- Add architecture info here -->

While all 3 environments may not be required for a specific client pilot, to complete the Level 4 learning plan and earn the {{badge.name}} badge, you will need to provision all three ITZ environments and complete all the sections in the lab guide. Ignore any statements in the ITZ collection regarding **optional** environment or tasks. 

 <!-- ITZ currently restricts individual users to a maximum of two concurrent environment reservations for the purpose of education or training. To overcome this limitation, an IBM Sales Cloud opportunity number has been created: #######. You will use this number when creating your reservations. This opportunity number should only be used for the purpose of completing this training. -->

Follow the instructions in this section to create new reservation requests, extend the reservations, and access the ITZ demonstration environments.
## Create a reservation request
1. Click each of the links to open a browser to the reservation pages of the **{{itz.collectionName}}**.

    !!! Warning "You may be asked to authenticate to IBM Technology Zone"

        The steps to authenticate to ITZ are not detailed here as they may vary between users.

    <a href="{{itz.orchestrateEnv}}" target="_blank">watsonx Assistant for Z lab – watsonx Orchestrate - reservation page</a>
    
    <a href="{{itz.aapEnv}}" target="_blank">Ansible Automation Platform (AAP) & z/OS - reservation page</a>
    
    <a href="{{itz.snoEnv}}" target="_blank">Single Node OpenShift with NFS storage - reservation page</a>

!!! Important "Images below are for 1 of the 3 environments"

    **Be sure to follow these steps to create a reservation in ITZ for all three environments!**

2. Click **Reserve now**.

    The **Reserve now** option creates a reservation for immediate use. Optionally, schedule the reservation for a later date, like when you will be at your client's office.

    ![](_attachments/itzRSVPReserveNow.png)

3. Complete the reservation request and click **Submit**.

    The first two reservations will be similar to the first image below and and have fields **a**-**e** that will need to be completed.

    **a**. Optionally, change the **Name** field for the reservation.

    **b**. Select the **Education** purpose tile.

    **c**. Enter a **Purpose description**.

    **d**. Select the region nearest your physical location in the **Preferred Geography** drop-down.

    **e**. The **End date and time** will be set to 2 days after the current date and time.

    **f**. Accept the IBM Technology Zone's terms and conditions and security policies.

    **g**. When satisfied with the parameters, click **Submit**.

    ![](_attachments/itzRSVPReservationPage.png)

    In addition to the above fields, the reservation for the **Single Node OpenShift with NFS storage** will have these additional fields:

    **h**. Leave the default setting of **10.128.0.0/14**.

    **i**. Leave the default setting of **No**.

    **j**. Select **16 vCPU x 64 GB - 300 GB ephemeral storage** in the **Master Single Node Flavor** pull-down menu.

    **k**. Select **4.14** in the **OpenShift Version** pull-down menu.

    **l**. Leave the default setting of **172.30.0.0/16**.

    ??? Warning "If your reservation for the Single Node OpenShift environment fails..."

        If your reservation for the Single Node OpenShift environment fails, try selecting one of the **eu-gb region** options as the **Preferred Geography**.

    ![](_attachments/itzRSVPReservationPage2.png)

<div style="page-break-after: always;"></div>

## Extend the reservation
During the provisioning process, multiple emails are sent to you from ITZ as the provisioning process runs. One email states the reservation is provisioning and the other email states that the environment is **Ready**.

In rare cases, the provisioning process may fail. If you receive an email stating the reservation failed, try again by repeating Steps 1-3 for the environment that failed to provision. If issues continue, open an ITZ support ticket using the methods mentioned in the [Support](index.md#support) section of this guide.

When the reservations are in the **Ready** state, you can extend each reservation to a total of 6 days. Remember, IBM sellers need the environment to record their Stand and Deliver and Business Partners need an environment to answer quiz questions. Plan your time accordingly.

4. In the IBM Technology Zone portal, expand **My TechZone** at the top and select **My Reservations**.

    ![](_attachments/itzMyReservations.png)

5. Click the **overflow icon** (![](_attachments/overflowIcon.png)) on the reservation tile and select **Extend**.

    ![](_attachments/itzExtendMenu.png)
<div style="page-break-after: always;"></div>

6. Click the **Select a date** option, (a) specify the date to extend to, and then (b) click **Extend**.

    ![](_attachments/itzExtendRsvp.png)

If you anticipate needing more time, repeat Steps 5 and 6 to extend the reservation to the maximum of 6 days.
<div style="page-break-after: always;"></div>

## Join the ITZ IBM Cloud accounts
Both the **watsonx Assistant for Z lab – watsonx Orchestrate** and the **Ansible Automation Platform (AAP) & z/OS** environments include adding you to an IBM Cloud account while your reservation is active. During the provisioning process of the ITZ environments, you should receive two emails. In order to access the environment, you must first accept the invitations to join both of the IBM Cloud accounts. 

7. Open the emails from **IBM Cloud** and click the **Join now** links.

    ![](_attachments/itzJoinCloudEmail.png)

8. In the **Join IBM Cloud** browser windows that open, select the **I accept the product Terms and Conditions of this registration form, and then click **Join Account**.

    ![](_attachments/itzJoinCloud.png)

**Repeat steps 7 and 8 for the second invitation.**

After joining both accounts, verify both accounts appear in your available account list in the IBM Cloud portal.

9. Click the link below to open a browser to the IBM Cloud Portal.

    <a href="cloud.ibm.com" target="_blank">**IBM Cloud portal**</a>

10. Follow the directions to complete the authentication to IBM Cloud using the same email address you used to login to ITZ. The login steps may very depending on any two-factor authentication methods enabled. 

    ![](_attachments/itzCloudLogin.png)

11. Click the **account** menu and verify access to the two IBM Cloud accounts: **{{itz.account1}}** and **{{itz.account2}}**

    !!! Important "These accounts may change within ITZ."

        Over time, the accounts may change for the environments. The accounts names should align with the accounts named in the invitation emails you received. 

    ![](_attachments/itzCloudAccountsVerify.png)

    ??? Tip "Does your IBM Cloud portal view look different?"

        If your IBM Cloud portal looks different from the images above, it could be because the IBM Cloud portal has done through a design change, or your browser window is set to smaller size. Instead of the current selected account appearing in the top menu, you may see this **change account** icon: ![](_attachments/itzCloudChangeAccountIcon.png). Click this icon to view the list of accounts you can access.

        ![](_attachments/itzCloudChangeAccountIconBig.png)

## Accessing the environments
Each reservation provides access to its respective environment. Details for accessing each environment are provided in the **Pilot setup** sections that follow in the lab guide.

Proceed to the next section to perform the pilot setup steps.