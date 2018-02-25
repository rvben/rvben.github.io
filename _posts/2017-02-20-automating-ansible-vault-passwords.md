---
layout: post
title:  "Automating Ansible Vault Passwords"
date:   2017-02-20 19:00:00 +0200
categories: ssh jenkins ci expect
---
# Using Expect to Automate Ansible Vault Passwords

There are a number of scenario's in which you'd want to automate the input of a Ansible Vault password.
In my case, it was needed in a Jenkins job.

The first choice is obvious. Ansible provides the possible to use a vault-password.
A second one is with the tool 'expect'.

## Password-file
When automating passwords from Jenkins, you often want to get them from environment variables.
If you want to use a password file to achieve this, you'd end up with something like this:
```
echo $password > .vaultpassfile
sleep 1 && rm -f .vaultpassfile &
ansible-playbook playbook.yml --vault-password-file=.vaultpassfile

```
### Cons
The big con is that there will be a password-file with your password in plain text.
Although the file has a short life, it doesn't feel nice.


## Expect
You can also automate it using expect.
The challenge was to return the proper return-code from the process.

```
expect << EOF
set timeout -1
spawn ansible-playbook playbook.yml --ask-vault-pass
expect "Vault password:"
send "$password\r"
expect eof
catch wait result
exit [lindex \$result 3]
EOF
```

This one is a bit longer, but ensures that you don't need a temporary password file.
