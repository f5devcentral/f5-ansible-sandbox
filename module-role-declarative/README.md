## Description
Example ansible environment which includes 3 playbooks which accomplish a similar task using different approaches. The intent is to highlight the benefits of abstraction when utlilizing automation tools. The examples here are meant as a starting point to build your automation story and not as a comprehensive solution.

#### Dependencies
The following extensions are needed only for the declarative playbook:
* Application Services 3 [AS3](https://clouddocs.f5.com/products/extensions/f5-appsvcs-extension/latest/)
* Declarative Onboarding [DO](https://clouddocs.f5.com/products/extensions/f5-declarative-onboarding/latest/)

#### Playbook Overviews
* **gslb_modules.yaml** creates an F5 GSLB object using modules (no app or loop logic built in).
* **gslb_roles.yaml** creates the same F5 GSLB object using the [bigip_gslb](https://galaxy.ansible.com/f5devcentral/bigip_gslb) role hosted on **Ansible Galaxy**.
* **gslb_declarative.yaml** creates the same F5 GSLB object, deploys an L4-L7 ADC config, and onboards (L1-L3) the same BIG-IP appliance using the F5 Automation Toolchain declarative API extensions.


## Setup
1. Edit the **hosts** file to represent your environment.
1. Update `host_vars/..` with the connection details for your hosts.
    1. Ensure that the password is encrypted with vault when used outside of this demo.
       ```
       ansible-vault encrypt_string admin --ask-vault-pass --name f5_password
       ```
1. Create var files that represent your application ADC parameters. In this demo the example app vars are within `apps/`

## Notes
* Both the modules and roles playbook take **extra-vars** inline to define which application var file you want to use like in the example below.
    ```
    ansible-playbook gslb_roles.yaml -e "@apps/app1.yaml"
    ```
* The **declarative** playbook references var files within this folder structure to feed the JSON payload of the API calls. You will need to update these files/paths according to your setup.
