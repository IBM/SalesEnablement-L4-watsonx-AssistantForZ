
# SalesEnablement-L4-watsonx-AssistantForZ
Sales Enablement Level 4 for watsonx Assistant for Z training for IBMers and BPs.

**Last publish date:** October 30, 2024

## Summary of environment and automation

The ITZ collection and associated environment for this L4 was created and is maintained by Max Weiss. All resources provisioned in the demo environment are shared amongst all users. See support slack channel for more assistance.

**ITZ Collection:** <a href="https://techzone.ibm.com/collection/6633e75d979046001eea2b77" target="_blank">https://techzone.ibm.com/collection/6633e75d979046001eea2b77</a>

## Maintenance

Use the slack support channel or contact Max and/or his team.

    supportSlack: 
        name: "#watsonx-assistant-z-technical"
        url: "https://ibm.enterprise.slack.com/archives/C07ARLXF2R1"

## Building the demonstration guide

Three step process for this one:


1. Build all the html output from the markdown

    ```
    mkdocs build
    ```

2. Generate the PDF file. Local mkdocs test server needs to be running (```mkdocs serve```).

    ```
    node export_to_pdf.js http://localhost:8000/print_page/index.html docs/_pdf/'IBM watsonx Assistant for Z Level 4 Lab Guide.pdf' 'IBM watsonx Assistant for Z Level 4 Lab Guide'
    ```

Next, review PDF and make sure it is as clean as possible with regard to line breaks. If needed, modify the markdown files as necessary. Use ```<div style="page-break-after: always;"></div>``` to split output to a new page, but do not put the break within an indented section or the generated markdown will not format properly. If changes are made, repeat steps 1 and 2 above. 

!!! Warning "The PDF may be too large for github!"

    Use adobe acrobat to compress the PDF file created above.

3. Deploy the content.

    ```
    mkdocs gh-deploy
    ```

4. Sync all the changes to the github repository.