# Security
- secure access to services and infrastructure resources
    - network access controls
    - identity and access privileges
- encrypt in transit and stored data
- monitor & alert:
    - user activity
    - network traffic
    - cloud & network configuration
    - in all environments
- identify vulnerabilities in:
    - application code (static code analysis)
    - build artifacts
    - VM & Container images
    - operating systems
    - cloud & networking configuration

### RSA & GPG
- `ssh-keygen -t rsa -C user@email`
    - user in comment should match the user to login to remote system

---
## Server Security Best Practices
- keep instances up to date and patched
- enforce hardening
- add vulnerability monitoring
- implement monitoring and alerting tools


---
## Securing a Web Server
[SSL Test](https://www.ssllabs.com/ssltest/analyze.html)

### Debian
- fail2ban

#### Firewall
- ufw

---
## Secrets
- sealed secrets: encrypted secrets stored in repositories

### Vault
- an alternative to storing secrets as encrypted text in repositories that offers additional features beyond secure secret storage:
    - dynamic secrets: generating secrets for on-demand access
    - data encryption: encrypt and decrypt data in databases without Vault actually storing it
    - leasing and renewl: associates a lease with all secrets that provides the ability to renew the leases or revoke the secret if expired
    - revocation: revoke single secrets or a tree of secrets such as all secrets read by a specific user or of a particular type (useful for key rolling and locking down systems)
    - monitor access

###### Setup
1. Install

- Manual: download vault binary and adding it to PATH
- Mac (Homebrew): brew install vault
- Windows (Chocolately): choco install vault

2. Start a DEV server
- `vault server -dev`
- NOTE: Never run the DEV server in production!

3. Save configuration

- Save the generated unseal key into a new file called "unseal.key"
- Add environment variables:
    - `export VAULT_ADDR="http://127.0.0.1:8200"`
    - `export VAULT_DEV_ROOT_TOKEN_ID="s.TolAAwSmTYlBmE0E1M4M0ADG"`

4. Check server satuts
- `vault status`

###### Add secrets
`vault kv put secret/hello foo=world excited=yes`

###### Get secrets
- `vault kv get secret/hello`
- `vault kv get -field=excited secret/hello`
- `vault kv get -format=json secret/hello | jq -r .data.data.excited`

###### Delete secrets
`vault kv delete secret/hello`


---
## Network Traffic

### Ingress
- can be controlled and restricted using network ACLs, security groups, which are both effective firewalls, routing rules, and host based endpoint security tools which oftentimes contain firewall capabilities.

### Egress
- handled using internet gateways and nat gateways. As with ingress traffic, egress traffic should also be controlled and restricted for a number of reasons.

---
## Security Scripts
```
 #!/bin/bash
 #
 #   attacker - prints out the last failed login attempt
 #
 echo "The last failed login attempt came from IP address:"
 grep -i "disconnected from" /var/log/auth.log|tail -1| cut -d: -f4| cut -f7 -d" "
 ```

 ```
 #
 ##  	topattack - list the most persistent attackers
 #
 if [ -z "$1" ]; then
 echo -e "\nUsage: `basename $0` <num> - Lists the top <num> attackers by IP"
 exit 0
 fi
 echo " "
 echo "Persistant recent attackers"
 echo " "
 echo "Attempts   	IP "
 echo "-----------------------"
 grep "Disconnected from authenticating user root" /var/log/auth.log|cut -d: -f 4 |
 ```


---

## Kali Linux