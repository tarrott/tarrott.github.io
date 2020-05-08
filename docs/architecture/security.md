# Security

### RSA & GPG
- `ssh-keygen -t rsa -C user@email`
    - user in comment should match the user to login to remote system

---
## Securing a Web Server
[SSL Test](https://www.ssllabs.com/ssltest/analyze.html)

### Debian

#### Firewall


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

## Kali Linux