# Getting started with skills and actions
Watsonx Assistant for Z can import skills to automate a range of IBM Z related tasks through assistant interactions. Each skill is a pre-defined automation that runs tasks to accomplish some unit or units of work. For example, you can use a skill to view z/OS IPL information, or work with z/OS datasets.

IBM watsonx Assistant for Z provides an extension within watsonx Orchestrate to build new skills from Ansible Automation platform or z/OS Management Facility (z/OSMF). The Z Skills Accelerator extension connects Ansible and z/OS application programming interfaces (APIs) and can import automation as Ansible Playbooks, JCL, or REXX as skills. Learn more importing and building skills <a href="https://www.ibm.com/docs/en/watsonx/waz/2.x?topic=building-skills-from-ansible-controller-zos" target="_blank">here</a>.

## Environments
### Watsonx Orchestrate
The Z Skills Accelerator extension is already configured in your watsonx Orchestrate IBM Technology Zone (ITZ) environment. You can use this component to import new skills.

### Ansible Automation Platform and Wazi as a Service
To import skills for automations, you use Ansible Automation Platform (AAP) and Wazi as a Service (Wazi aaS) to serve as the z/OS back-end. Learn more about AAP <a href="https://www.redhat.com/en/technologies/management/ansible" target="_blank">here</a>. Learn more about Wazi, <a href="https://www.ibm.com/cloud/wazi-as-a-service" target="_blank">here</a>.

The two resources are provisioned together in the ITZ environment that you reserved earlier. This environment enables the ability to manage and automate z/OS tasks and subsystems with various preinstalled ansible playbooks. It includes a z/OS back-end (Wazi as a Service) with all needed prerequisites.

The playbooks provided cover various use cases for automating z/OS management. Ansible’s capabilities for automating various Z-specific tasks are not limited to the use cases that are preinstalled in the AAP instance. The preinstalled playbooks are tasks from the ‘IBM z/OS core collection’. Using this environment accelerates the ability to showcase the value of watsonx assistant for Z, and to get started with simple automations that can be expanded.

The ITZ environment gives you access to AAP, which is preconfigured to target the accompanying z/OS Wazi system, along with web-based access to AAP to experiment with different playbook templates. These templates are imported into watsonx Orchestrate as skills and connected to your assistant.

For more information on the AAP and Wazi z/OS environments, refer to this <a href="https://ibm.ent.box.com/v/ansible4zos-demo-guide" target="_blank">document</a>. 

The playbook templates that are preinstalled in AAP cover various use cases, which you can explore, including:

- z/OS Certificate Management (create, delete, list, and renew certificates)
- dataset management (create, delete, fetch datasets)
- Submit JCL
- Run Operator commands
- Run TSO commands
- And more

Each of the sections that follow build upon each other. Complete each to successfully enhance your assistant by starting with [Explore Ansible Automation Platform](exploreAAP.md).