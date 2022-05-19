Setup a Papyrus instance quickly with Vagrant and Ansible. The Ansible playbooks can also be used to deploy to a real production server in a similar fashion.


## Getting started

```
git clone https://github.com/yorkulibraries/vagrant-papyrus.git
cd vagrant-papyrus
```

Edit vars/app.yml and set the app_domain variable to your domain eg: me.ca

Edit other variables in the vars folder to match your environment.

Clone the ansible-rails project and bring up the vagrant instance.
```
git clone https://github.com/yorkulibraries/ansible-rails.git
vagrant up
```

Watch for any error/failed tasks. If all is good then the instance is ready to use for testing.

Apache auth_basic is used for Basic Authentication. The default administrator username/password is:

```
admin/papyrus
```

Edit /etc/hosts and add an entry like followed so you can access the app from a browser at http://papyrus.me.ca/

```
192.168.168.168 papyrus.me.ca
```

## Provisioning Papyrus on a remote server/VM

You can deploy Papyrus on a remote server or VM using the papyrus_provision.yml playbook similar to the command below:

```
ansible-playbook -i inventory papyrus_provision.yml --limit target_host
```


## About Papyrus
Take a look at [Papyrus](https://github.com/yorkulibraries/papyrus) repo for Papyrus code.
