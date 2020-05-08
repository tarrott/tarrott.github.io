# CICD

---
## Build

### Jenkins

---
## Test

### Unit Tests

### Integration Tests

### End-to-end Tests

---
## Deploy
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
