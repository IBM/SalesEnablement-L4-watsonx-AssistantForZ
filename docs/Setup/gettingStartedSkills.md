# Getting started with skills and actions
Watsonx Assistant for Z provides the ability to import skills to automate a range of IBM Z related tasks through assistant interactions. Each skill is a pre-defined automation that performs a different task to accomplish some work. For example, you can use a skill to view z/OS IPL information, or work with z/OS datasets.

IBM watsonx Assistant for Z provides an extension within watsonx Orchestrate that allows you to build new skills from Ansible Automation platform or z/OS Management Facility (z/OSMF). This extension, Z Skills Accelerator, allows you to connect to Ansible and z/OS application programming interfaces (APIs) and enables you to import automation as Ansible Playbooks, JCL, or REXX as skills. Learn more importing and building skills <a href="https://www.ibm.com/docs/en/watsonx/waz/2.x?topic=building-skills-from-ansible-controller-zos" target="_blank">here</a>.

## Environments
### watsonx Orchestrate
The extension for watsonx Assistant for Z that allows for importing skills from Ansible Automation platform (AAP) or z/OSMF is called the ‘z/OS Skills Accelerator’ which is already configured in your watsonx Orchestrate IBM Technology Zone (ITZ) environment. You will be using this component when it comes to importing new skills.

### Ansible Automation Platform (AAP) and Wazi as a Service (aaS)
To import skills for automations, you will be using Ansible Automation Platform (AAP) and Wazi aaS to serve as the z/OS back-end. Learn more about AAP <a href="https://www.redhat.com/en/technologies/management/ansible" target="_blank">here</a>. Learn more about Wazi, <a href="https://www.ibm.com/cloud/wazi-as-a-service" target="_blank">here</a>.

The two resources are provisioned together in the ITZ environment you reserved earlier. This environment will enable the ability to manage and automate z/OS tasks and subsystems with a variety of pre-loaded ansible playbooks ready for immediate use, as well as z/OS installed with all prerequisites needed (no additional setup required).

The playbooks provided cover a variety of simple use cases for automating z/OS management. Ansible’s capabilities for automating various Z-specific tasks are not limited to the use cases that are pre-loaded in the AAP instance. These are mostly tasks from the ‘IBM z/OS core collection’. Leveraging this environment will accelerate the ability to showcase the value of watsonx assistant for Z when it comes to automation, and to get started with simple automations that can be expanded.

The ITZ environment gives you access to AAP which is preconfigured to target the accompanying z/OS Wazi system, along with web-based access to AAP to experiment with different playbook templates. These templates will be imported into watsonx Orchestrate as skills and connected to your assistant.

For more information on the AAP & Wazi z/OS environments, refer to this <a href="https://ibm.ent.box.com/v/ansible4zos-demo-guide" target="_blank">document</a>. 

The playbook templates pre-loaded into AAP cover various use cases which you will be able to explore, including:

- z/OS Certificate Management (create, delete, list and renew certificates)
- dataset management (create, delete, fetch datasets)
- Submit JCL
- Execute Operator commands
- Execute TSO commands
- And more

Each of the sections that follow build upon each other. Complete each in order to successfully enhance your assistant starting with [Explore Ansible Automation Platform](exploreAAP.md).