---
layout: blog
title:  "Using Expect to Automate SSH Passphrase Input"
date:   2017-01-02 20:00:00 +0200
categories: ssh jenkins ci
---
# Using Expect to Automate SSH Passphrase Input

There are a number of scenario's in which you'd want to automate the input of a SSH Passphrase.
In my case, it was needed in a Jenkins job.
Because, how handy would it be to read a passphrase from Jenkins credentials, or secret environment variables?
Very.

Unfortunately, 'ssh-add' is not tailored for such situations.
It spawns a seperate tty, so piping the passphrase into ssh-add won't be possible.

Luckily, there is this tool called 'expect', which is able to interact and automate such tty.
My final solution looked like this, where the environment variable containing the passphrase is called 'SSH_PASS'.

So expect spawns the tty by calling 'ssh-add', waits for the prompt, and fills in the passphrase. Easy as that.

```
eval $(ssh-agent -s)
expect << EOF
  spawn ssh-add /var/lib/jenkins/.ssh/id_rsa
  expect "Enter passphrase"
  send "$SSH_PASS\r"
  expect eof
EOF
```
