Ansible templates & tasks for RethinkDB
=======================================

This repository contains some ansible tasks & templates designed to help get you started building a production-ready rethinkdb server.

Functionality includes:

 - Provisioning fully working RethinkDB server
 - Automatic restart upon crash via upstart (hopefully RethinkDB will never crash though :wink:!)
 - Password-protected RethinkDB Administration Console via nginx reverse proxy
 - Automatic backups to s3 every hour

The tasks contained within this repo are designed to be run in a play that has the following vars:

 - `authkey`
 - `awsAccessKey`
 - `awsSecretKey`

You can either set the authkey var yourself such as:

```yaml
- name: Setup just-provisioned RethinkDB Server
  hosts: ec2hosts
  sudo: yes
  gather_facts: yes
  remote_user: ubuntu
  vars:
    awsAccessKey: XXX
    awsSecretKey: YYY
    authkey: ZZZ
  tasks:
  - include: install-rethinkdb.yml
```

... or have Ansible create an authkey automatically for you:

```yaml
- name: Setup just-provisioned RethinkDB Server
  hosts: ec2hosts
  sudo: yes
  gather_facts: yes
  remote_user: ubuntu
  vars:
    awsAccessKey: XXX
    awsSecretKey: YYY
    authkey: "{{lookup('password', '/tmp/passwordfile chars=ascii_letters,digits,hexdigits')}}"
  tasks:
  - include: install-rethinkdb.yml
```

This is designed to run on Ubuntu 14.04 LTS within the AWS cloud but most likely works on other distros/versions/clouds as well. If nothing else it's a good starting point for your own tasks/plays.

Pull requests welcome!!
