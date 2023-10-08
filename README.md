# Ansible role: labocbz.install_portsentry

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Cbz-blue)

## Description

![Tag: Portsentry](https://img.shields.io/badge/Tech-Portsentry-orange)
![Tag: Iptables](https://img.shields.io/badge/Tech-Iptables-orange)

An Ansible role to install and configure Portsentry on your host.

The Ansible role installs and configures Portsentry, a security tool used to detect and block port scans and suspicious activities on a system. The role offers the flexibility to define base addresses and whitelist specific addresses, ensuring trusted sources are not blocked by Portsentry. The configuration of excluded ports is managed through a pre-existing non-templated file available within the role's source code (files/ directory).

By deploying Portsentry with this role, administrators can enhance the security of their systems by proactively detecting and mitigating potential attacks. The ability to customize base addresses and whitelist rules empowers administrators to tailor the role to their specific network environment, ensuring legitimate traffic is not mistakenly blocked.

The role provides a streamlined approach to set up and configure Portsentry effectively, while the management of excluded ports through a pre-existing configuration file ensures consistency and ease of adaptation between deployments.

In summary, the Portsentry role offers a robust solution to bolster system security against port scans and suspicious activities, empowering administrators with customizable options to accommodate their network requirements while avoiding the blocking of trusted sources.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
portsentry_base_addresses:
  - "127.0.0.1/32"
  - "0.0.0.0"

portsentry_ignore_addresses:
  - "192.168.1.1/24"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_portsentry_base_addresses:
  - "127.0.0.1/32"
  - "0.0.0.0"

inv_portsentry_ignore_addresses:
  - "192.168.1.1/24"
  - "192.168.1.2/24"
  - "192.168.1.3/24"
  - "192.168.1.4/24"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_portsentry"
    tags:
    - "labocbz.install_portsentry"
    vars:
    portsentry_base_addresses: "{{ inv_portsentry_base_addresses }}"
    portsentry_ignore_addresses: "{{ inv_portsentry_ignore_addresses }}"
    ansible.builtin.include_role:
    name: "labocbz.install_portsentry"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-04-27: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
