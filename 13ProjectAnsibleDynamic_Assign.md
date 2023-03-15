# PROJECT-13-Ansible-Dynamic-Assignments-include-and-community-Roles


### Ansible Dynamic Assignments (Include) and Community Roles to create a LAMP Server

Ansible Dynamic Assignments refer to the process of dynamically assigning values to variables, groups, and hosts in Ansible. This is useful when you need to manage a large number of hosts or when the inventory information is constantly changing.<br></br>


In this project, the UAT servers will be configured to learn about and practice using new Ansible concepts and modules.
This project will introduce dynamic assignments by using the include module.
The static assignments from [Project 12](https://github.com/Rietta1/DevOps-Projects/blob/main/12ProjectAnsibleRefactoring.md) use the import Ansible module. The module that enables dynamic assignments is include.


```
import = Static
include = Dynamic
```

When the import module is used, all statements are pre-processed at the time playbooks are parsed. When site.yml playbook is executed, Ansible will process all the playbooks referenced during the time it is parsing the statements. <br>This also means that, during actual execution, if any statement changes, such statements will not be considered. Hence, it is static.

On the other hand, when include module is used, all statements are processed only during execution of the playbook. After the statements are parsed, any changes to the statements encountered during execution will be used.

*Note: For most projects, static assignments are preferred as they are more reliable. Dynamic assignments can be challenging in the debugging process. However, they can be useful for environment specific variables as in this project.*

#### Introducing Dynamic Assignment Into the structure

In the ```https://github.com/<your-name>/ansible-config-mgt GitHub repository ```, start a new branch and call it dynamic-assignments.
  
![project13ad](https://user-images.githubusercontent.com/41236641/156893444-1afded89-2ff9-47ba-ba99-b2ea2f6bb85a.PNG)



Create a new folder, name it dynamic-assignments. Then inside this folder, create a new file and name it env-vars.yml. site.yml will be instructed to include this playbook later. 

GitHub will have the following structure at this point:
```
├── dynamic-assignments
│   └── env-vars.yml
├── inventory
│   └── dev
    └── stage
    └── uat
    └── prod
└── playbooks
    └── site.yml
└── roles (optional folder)
    └──...(optional subfolders & files)
└── static-assignments
    └── common.yml
```

Since Ansible will be used to configure multiple environments and each of these environments will have certain unique attributes, such as servername, ip-address etc., there has to be a way to set values to variables per specific environment.

Create a folder to keep each environment’s variables file and name it as env-vars. Then for each environment, create new YAML files which will be used to set variables.

The layout should now look like this:
```
├── dynamic-assignments
│   └── env-vars.yml
├── env-vars
    └── dev.yml
    └── stage.yml
    └── uat.yml
    └── prod.yml
├── inventory
    └── dev
    └── stage
    └── uat
    └── prod
├── playbooks
    └── site.yml
└── static-assignments
    └── common.yml
    └── webservers.yml
```

![project13dir](https://user-images.githubusercontent.com/41236641/156893574-d0d6f450-74fe-4286-9556-9638f25b4c23.PNG)


Paste the instruction below into the env-vars.yml file:
```yml
---
- name: collate variables from env specific file, if it exists
  include_vars: "{{ item }}"
  with_first_found:
    - "{{ playbook_dir }}/../env_vars/{{ "{{ inventory_file }}.yml"
    - "{{ playbook_dir }}/../env_vars/default.yml"
  tags:
    - always
```

![project13solution](https://user-images.githubusercontent.com/41236641/156893603-52304f80-7081-4305-a478-af0c9e62e433.PNG)


Three things to note from the above code:

The 'include_vars' syntax has been used instead of include. This is because Ansible developers decided to separate different features of the module. From Ansible version 2.8, the include module is deprecated and variants of include_* must be used. These are:

```
include_role
include_tasks
include_vars
```
In the same version, variants of import were also introduced, such as:

```
import_role
import_tasks
```

Special variables {{ playbook_dir }} and {{ inventory_file }} are used in the above code. {{ playbook_dir }} will help Ansible to determine the location of the running playbook, and from there navigate to other path on the filesystem. {{ inventory_file }} on the other hand will dynamically resolve to the name of the inventory file being used, then append .yml, so that it picks up the required file within the env-vars folder.

The variables are included using a loop. with_first_found implies that, looping through the list of files, the first one found is used. This is good practice so that the default values can be set in case an environment specific env file does not exist.


#### Update site.yml with dynamic assignments

Update site.yml file to make use of the dynamic assignment. (At this point, testing cannot be done yet. This is just setting the stage for what is to come).

site.yml should now look like this:
```yml
---
- name: Include dynamic variables 
  hosts: all
  tasks:
    - import_playbook: ../static-assignments/common.yml 
    - import_playbook: ../dynamic-assignments/env-vars.yml
  tags:
    - always

- name: Webserver assignment
  hosts: webservers
    - import_playbook: ../static-assignments/webservers.yml
```

![Project13dir2](https://user-images.githubusercontent.com/41236641/156893696-4797c629-acaf-4b6a-81a1-e8941d2f3802.PNG)


#### Community Roles

It is time to create a role for MySQL database - it should install the MySQL package, create a database and configure users. There are tons of roles that have already been developed by other open source engineers. These roles are actually production ready, and dynamic to accomodate most of Linux flavours. With Ansible Galaxy again, we can simply download a ready to use ansible role. 

#### Download Mysql Ansible Role

Browse the available community roles [here](https://galaxy.ansible.com/home)

This project will make use of a MySQL role developed by [geerlingguy](https://galaxy.ansible.com/geerlingguy/mysql).

On Jenkins-Ansible server make sure that git is installed with git --version, then go to ‘ansible-config-mgt’ directory and run:

```
git init
git pull https://github.com/<your-name>/ansible-config-mgt.git
git remote add origin https://github.com/<your-name>/ansible-config-mgt.git
git branch roles-feature
git switch roles-feature
```

Inside roles directory, create a new MySQL role with ansible-galaxy install geerlingguy.mysql and rename the folder to mysql:

```
ansible-galaxy install geerlingguy.mysql
mv geerlingguy.mysql/ mysql
```

Read README.md file, and edit roles configuration to use correct credentials for MySQL required for the tooling website. The main.yml file in the defaults folder of roles directory is edited as follows:

![project13roles](https://user-images.githubusercontent.com/41236641/156893823-3669ca8e-627d-4432-b853-75abbd2663fd.PNG)

Now it is time to upload the changes into GitHub:

```
git add .
git commit -m "Commit new role files into GitHub"
git push --set-upstream origin roles-feature
```

Create a Pull Request and merge it to main branch on GitHub.

#### Load Balancer roles

In order to be able to choose which Load Balancer to use, Nginx or Apache, we will need to have two roles:

1. Nginx
2. Apache

- With the experience gained from the projects on Ansible so far, it is understood that roles could either be developed or recreated easily from available roles in the community. 

![Project13ade](https://user-images.githubusercontent.com/41236641/156893897-a38e917d-a222-4d50-818c-7e1991b2e929.PNG)

![Project13add](https://user-images.githubusercontent.com/41236641/156893925-0eb9f2df-4f8f-45b8-8584-c151ec438e97.PNG)


- After creating the roles, update both static-assignment and site.yml files to refer the roles.

Important Hints:

- Since both Nginx and Apache load balancer cannot be used together, add a condition to enable either one - this is where the use of variables is handy.
- Declare a variable in defaults/main.yml file inside the Nginx and Apache roles. Name each variables enable_nginx_lb and enable_apache_lb respectively.
- Set both values to false like this enable_nginx_lb: false and enable_apache_lb: false.
- Declare another variable in both roles load_balancer_is_required and set its value to false as well
- Update both assignment and site.yml files respectively

On main.yml in Nginx:

![Project13main](https://user-images.githubusercontent.com/41236641/156894003-44605a42-e98c-4e52-a0c7-597bcd9b9836.PNG)

On main.yml in Apache:

![project13main1](https://user-images.githubusercontent.com/41236641/156894034-3b77dc72-13c9-44f0-a400-bc0339e536b5.PNG)

Create loadbalancers.yml file in static-assignments folder and copy the following code:
```
- hosts: lb
  roles:
    - { role: nginx, when: enable_nginx_lb and load_balancer_is_required }
    - { role: apache, when: enable_apache_lb and load_balancer_is_required }
```

Update site.yml file with the following code:
```
- name: Loadbalancers assignment
       hosts: lb
         - import_playbook: ../static-assignments/loadbalancers.yml
        when: load_balancer_is_required 
```

![Poject13site](https://user-images.githubusercontent.com/41236641/156894116-55e7b85b-4058-4bb6-aace-56a064bb917d.PNG)


Now the env-vars\uat.yml file can be used to define which loadbalancer to use in UAT environment by setting respective environmental variable to true.

- Activate load balancer, and enable nginx by setting these in the respective environment’s env-vars file.

![Project13env-vars](https://user-images.githubusercontent.com/41236641/156894178-801fad75-d3a2-4727-aa2f-0717dd0d6ca1.PNG)


- The same must work with apache LB, so it can be switched by setting respective environmental variable to true and other to false.

![Project13inv](https://user-images.githubusercontent.com/41236641/156894237-5bda667f-c655-481f-a521-c594bbff0d13.PNG)

- To test this, update inventory for each environment and run Ansible against each environment.

Blockers: The playbook was not running and I observed some errors in the env-vars.yml file. I updated the file with the following code:

```yml
---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - prod.yml
            - stage.yml
            - default.yml
          paths:
            - "{{ playbook_dir }}/env-vars"
      tags:
        - always
  ```
   ![project13solution](https://user-images.githubusercontent.com/41236641/156912057-790ae008-cdc0-4033-844b-0d90d04d0c31.PNG)

  After the above changes, the playbook executed successfully across all the servers. 
 ![project13playbook3](https://user-images.githubusercontent.com/41236641/156894287-7335ebe4-64f7-443c-8d6d-22286a4e75ea.PNG)
![Project13playbook4](https://user-images.githubusercontent.com/41236641/156894298-b0954544-c6ed-4764-b1f0-1898a1c97e1b.PNG)
![project13playbook5](https://user-images.githubusercontent.com/41236641/156894278-3a1c42ff-88c7-4058-b832-fcf2ee54b0cc.PNG)
![project13playbook6](https://user-images.githubusercontent.com/41236641/156894310-e736932e-8e37-46aa-b100-acfcb9acfc38.PNG)