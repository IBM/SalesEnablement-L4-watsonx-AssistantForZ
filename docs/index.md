Welcome to the {{guide.name}} (lab guide). This lab guide covers the setup, configuration, and usage of assistants for watsonx Assistant for Z. The lab guide is part of the {{learningplan.name}} learning plan for IBM and Business Partner Technical Sales and related badge. The lab guide also enables dedicated lab environments for customized client Proof of Experiences (PoX) and demonstrations. 


This lab guide leverages the <a href="" target="_blank">{{offering.name}} Velocity collection</a> and the 3 Velocity Pilot lab environments in IBM Technology Zone (ITZ). 

The lab guide provides guidance to:

- Provisioning the lab environments
- Creating an assistant and configuring conversational search
- Configuring assistant settings
- Testing conversational search
- Deploying a dedicated instance of OpenSearch for client document ingestion (Optional)
- Importing skills for z/OS automations
- Connecting apps to assistants
- Creating assistant actions
- Creating skill flows and custom-built actions
- Importing pre-packaged z/OS skills
- Publishing and deploying your assistant

Continue to the [Reserve the IBM Technology Zone environments](TechZoneEnvironment.md) section to begin the journey to obtaining the {{badge.name}} badge.

<div style="page-break-after: always;"></div>
## Support
Think something is down? Check the applicable status pages for any known issues like a site or service not available:

-  <a href="https://techzone.status.io/" target="_blank">IBM Technology Zone</a>

For issues with provisioning the ITZ environment for this lab (for example, a failed reservation request due to insufficient quota capacity) open a ticket with ITZ support:

- Web:  <a href="https://ibmsf.force.com/ibminternalproducts/s/createrecord/NewCase?language=en_US" target="_blank">IBM Technology Zone</a>

- Email: <a href="mailto:techzone.help@ibm.com" target="_blank">techzone.help.ibm.com</a>

For issues related to specific steps found in the demonstration guide after the ITZ environment is provisioned, contact the authors:

- Slack: <a href="mailto:{{supportSlack.url}}" target="_blank">{{supportSlack.name}}</a> - IBM only
- Email: <a href="mailto:{{supportEmail}}" target="_blank">{{supportEmail}}</a>

Business Partners should use the IBM Training live Chat Support service or other support methods that are found on the IBM Training portal <a href="https://ibmcpsprod.service-now.com/its?id=sc_category&sys_id=6568bfafdb2f13008ea7d6fa4b961990" target="_blank">here</a>.
<div style="page-break-after: always;"></div>
## Using the demonstration guide
Use these helpful tips to take full advantage of the {{guide.name}}.

??? tip "Printing the demonstration guide"

    !!! Warning "Printed or saved copies can be out of date"

        The {{guide.name}} changes regularly to match the {{offering.name}} offering and associated ITZ environment. Printed or saved copies of the demonstration guide can become out-of-date quickly and result in failed steps. 

    A ready-to-print PDF version of the {{guide.name}} is <a href="{{guide.pdf}}" target="_blank">here</a>. 

??? tip "Viewing images"

    Images in the demonstration guide can be enlarged by clicking on the image. Press the ++esc++ key or click the **X** to dismiss the enlarged image.
 
    <!-- ![](_attachments/testImage2.png) -->

??? tip "Image highlighting"

    In some images, the following styles of highlighting are used:

    - **Solid highlight box**: This style of box highlights where to click, enter, or select an item.
    ![](_attachments/welcome-1.png)

    - **Dash highlight box**: This style of box highlights one of two things: the path to follow to get to a specific location in the user interface, or areas to explore on your own.
    ![](_attachments/welcome-2.png)

??? tip "Copying commands and prompts"

    <!-- - **Copy to the clipboard**: The text is copied to the clipboard. Click the copy icon (highlighted) and then use the operating system paste function. For example, entering ```Ctrl+v```, ```Cmd+v```, or right-click and select ```Paste```.
    ![](_attachments/welcome-3.png) -->

    Copying and pasting commands and prompts from this demonstration guide is easy and can eliminate typographical errors.

    Click the highlighted copy icon and then use your operating system's paste function. For example, ++ctrl+v++, or right-click and select ```Paste```.
    ![](_attachments/welcome-3.png)

??? tip "Acronyms and terminology"

    IBM employees, and the tech industry in general, enjoy using acronyms. In the demonstration guide, most acronyms will appear with a dashed underline. Hover over the acronym to learn its meaning. A question mark (![](_attachments/questionIcon.png)) icon will first appear and after a second the tool tip with the acronym's meaning is displayed. Try it here: LPAR. 

    ![](_attachments/acronymHelp.png)

??? tip "The **Demonstration Guide** table of contents"

    This **Demonstration Guide** uses a responsive browser-based interface to ensure a pages are usable on various devices with different screen sizes. The Demonstration Guide table of contents may be displayed as highlighted in the green dashed box in this image:

    ![](_attachments/demoGuide1a.png)

    However, if the browser window is sized smaller, the table of contents can be accessed by clicking the main menu icon (![](_attachments/MainMenuIcon.png)):

    ![](_attachments/demoGuide2b.png)

    Click the main menu icon (![](_attachments/MainMenuIcon.png)) to expand the table of contents.

    <!-- ![](_attachments/demoGuide3.png) -->

