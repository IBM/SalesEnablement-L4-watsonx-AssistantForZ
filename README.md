# SalesEnablement-L4-watsonx-AssistantForZ
Sales Enablement Level 4 for watsonx Assistant for Z

# Doc build process:

mkdocs build

# mkdocs serve needs to be running for the node command below to work
node export_to_pdf.js http://localhost:8000/print_page/index.html docs/_pdf/'IBM watsonx Assistant for Z Level 4 Lab Guide.pdf' 'IBM watsonx Assistant for Z Level 4 Lab Guide'

# Use adobe acrobat to compress the PDF file created above.

mkdocs gh-deploy

# sync all the files back up to github
