# CICD

---
## Build

### Immutable Instances - Image Pipeline
1. Base OS image
2. Security hardening, patching, agents -> Hardened image
3. Application configuration -> Application Image
4. Security Scanning -> Released image
5. No changes performed on instances of the image prior to launching them in environments
6. Any further changes require building a new image and restarting the pipeline from step 1

- If immutable is not an option in the environment, then use a configuration management tool (Ansible) to track changes to the environment in a source version control repository

### Jenkins

---
## Test

### Unit Tests

### Integration Tests

### End-to-end Tests

---
## Deploy
Automated deployment characteristics:

- It can be triggered by just one action, like one command on the command line and it will do the job.
- The steps will be pre-defined, reproducible and predictable.
- There is little or no human intervention from the start to the end.
- It should show the deployment progress as it happens, better feedback
- It should be atomic, which means either all the steps are completed or nothing happens.

Some good to have features for automated deployment tools are:

- It should be able to deploy the same code to multiple servers
- Each deployment should be done from a given branch/tag/commit of a Version Control System (VCS) like git
- It should trigger notifications in the form of email/chat message
- Everyone should be able to view which branch/tag is deployed
- When a deployment is in progress, it should stop other deployments to start
- Rollback of the last deployment should be easy and fast.

[Source](https://dev.to/geshan/the-best-automated-deployment-tool-is-the-one-that-fits-your-needs-3o8)

- ansible
- octopus
- spinnaker

---
## Provision

### Infrastructure as Code

#### Terraform
- install terraform locally
  - download binary
  - download checksums & checksum signature
  - verify the signartue of the checksum against HashiCorp's GPG key
- add terraform binary to `PATH`

###### Create a VM on GCE example
1. Once per project enable the GCE API: `gcloud services enable compute.googleapis.com`
2. Create Terraform config
  - Run commands at creation: `metadata_startup_script` logs: `/var/log/daemon.log`
  - Add ssh keys: `metadata = { ssh-keys = ### }`
3. Run Terraform
  - terraform init
  - terraform plan
  - terraform apply
4. Clean up

Terraform config `main.tf`
```
resource "google_compute_instance" "default" {
  name         = "vm-${random_id.instance_id.hex}"
  machine_type = "f1-micro"
  zone         = "us-west1-a"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-9"
    }
  }

  # ...
}
```


#### Ansible
- [Ansible Control Machine (Dockerized)](https://github.com/jmal98/ansiblecm)
- `ansible-console` - REPL that comes with tab completion
- Access ENV variables: `{{ lookup('env','USER') }}"`
- `command` module escapes commands, while the `shell` module does not

###### Configuring VMs from Windows
1. Create a control node VM with debian or ubuntu
2. ssh into control node VM
3. `apt-get -y update && apt-get -y install ansible`

Note: run an installation script after creating a VM with Terraform
```
sudo apt update
sudo apt -y upgrade
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

4. Point to inventory dir
```
[defaults]
inventory = /vagrant/inv
host_key_checking = False
```


###### Docs
- [Ansible Docs](https://docs.ansible.com)
- [Ansible Galaxy Docs](https://galaxy.ansible.com/docs)
- `ansible-doc copy`
- `ansible-doc -t connection`
- `anisble-doc -t shell --list`
- `ansible-doc -t shell csh`
- list commands with `ansible` + tab

###### Inventory
- `ansible-inventory --graph --vars`
- `ansible-inventory --list`

*Inventory definitions in YAML*
all:
  hosts:
    ubuntu10:
      ansible_host: 192.168.50.10
      ansible_private_key_file: .vagrant/machines/ubuntu10/virtualbox/private_key

*Child group*
[ubuntu]
ubuntu10
ubuntu11
#ubuntu12

*Parent group*
[vagrant:children]
centos
ubuntu

[vagrant:vars]
ansible_user=vagrant
ansible_port=22


###### Ad-hoc command
- `ansible all -m ping`
- `ansible -m command -a "git config --global --list" vagrant`
- `ansible -m copy -a "src=master.gitconfig dest=~/.gitconfig" localehost`
- dry run: add `--check`
- show changes: add `--diff`
- show package managers: `ansible -m setup -a "filter=ansible_pkg_mgr" all`

###### Playbook
- `ansible-playbook playbook.yml`
- Debug: add `-v` up to `-vvvv`

```
name: Ensure ~/.gitconfig copied from master.gitconfig
hosts: localhost
tasks:
- name: copy git config
  copy:
    src: "mast.gitconfig"
    dest: "~/.gitconfig"
  when: ... # conditional
  become: yes # escalate privilege
  register git_config_copy # register output to variable

- name: print captured variable
  debug: var=git_config_copy
```
