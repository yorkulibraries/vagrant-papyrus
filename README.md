Setup a Papyrus instance quickly with Vagrant and Ansible. The Ansible playbooks can also be used to deploy to a real production server in a similar fashion.


## Getting started

The following steps to provision a *development* instance of Papyrus.  

Clone this repo:
```
git clone https://github.com/yorkulibraries/vagrant-papyrus.git
```

Change into the vagrant-papyrus directory:
```
cd vagrant-papyrus
```

Clone the ansible-rails project:
```
git clone https://github.com/yorkulibraries/ansible-rails.git
```

Clone the Papyrus project for development: (**NOTE:** we use SSH here to clone the Papyrus project because we want to be able to make changes.)
```
git clone git@github.com:yorkulibraries/papyrus.git
```

Bring up the box:
```
vagrant up
```

Watch for any error/failed tasks. If all is good then the instance is ready to use for testing.

Apache auth_basic is used for Basic Authentication. The default users for the 3 roles: Admin, Coordinator, and Student are:

```
admin/papyrus
coordinator/papyrus
student/papyrus
```

Edit /etc/hosts and add an entry like followed so you can access the app from a browser at http://papyrus.me.ca/

```
192.168.168.168 papyrus.me.ca
```

## Making changes

The directory **/home/papyrus/papyrus** in the box is a *symlink* to **/vagrant/papyrus**, which is a synced folder in the your local machine's **vagrant-papyrus** folder.
You can make changes on your local machine in **vagrant-papyrus/papyrus** folder and it is changed in the vagrant box too. 

## Running tests

SSH into the box as user **papyrus**
```
ssh papyrus@127.0.0.1 -p2222
cd papyrus
RAILS_ENV=test bundle exec rake db:reset
RAILS_ENV=test bundle exec rake test
```

## Provisioning Papyrus on a remote server/VM

You can deploy Papyrus on a remote server or VM using the papyrus_provision.yml playbook similar to the command below:

```
ansible-playbook -i inventory papyrus_provision.yml --limit target_host
```


## About Papyrus
Take a look at [Papyrus](https://github.com/yorkulibraries/papyrus) repo for Papyrus code.
