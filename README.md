Setup a [Papyrus](https://github.com/yorkulibraries/papyrus) instance quickly with Vagrant and Ansible. The Ansible playbooks can also be used to deploy to a real production server in a similar fashion.


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

Clone the [ansible-rails](https://github.com/yorkulibraries/ansible-rails) project:
```
git clone https://github.com/yorkulibraries/ansible-rails.git
```

Clone the Papyrus project for development: (**NOTE:** we use SSH here to clone the Papyrus project because we want to be able to make changes.)
```
git clone git@github.com:yorkulibraries/papyrus.git
```

Install the Ansible dependencies.

```
ansible-galaxy install -r requirements.yml
```

Bring up the box with RAILS_ENV=development (this is the default):

```
vagrant up
```

If you want to bring the box up with RAILS_ENV=production, then specify the environment variable when you run vagrant up:

```
RAILS_ENV=production vagrant up
```

Watch for any error/failed tasks. If all is good then the instance is ready to use for testing.

Apache auth_basic is used for Basic Authentication. The default users for the 3 roles: Admin, Coordinator, and Student are:

```
admin/papyrus
coordinator/papyrus
student/papyrus
apiuser/papyrus
```

Edit /etc/hosts and add an entry like followed so you can access the app from a browser at http://papyrus.me.ca/

```
192.168.168.168 papyrus.me.ca
```

## Set search API keys

Papyrus can search Worldcat and Alma/PrimoVE for bibliographic records. To make this work, you will need API keys for each of these services.

You can create a file with the search API keys (see vars/api_keys.yml for example), then run the playbook set_api_keys.yml to set them in the Papyrus database.

Note you must specify the rails_env variable to be the same as the one you have provisioned the box with.

If the file is encrypted, you will need to specify the Ansible Vault password. For example, at YUL, the file vars/yul_keys.yml is encrypted, you will run the following:

```
ansible-playbook set_api_keys.yml -e"app=papyrus rails_env=development apikeys=vars/yul_keys.yml" --ask-vault-pass
```

If the file is **not** encrypted and your API keys file is var/my_keys.yml, you will run the following:

```
ansible-playbook set_api_keys.yml -e"app=papyrus rails_env=development apikeys=vars/my_keys.yml"
```

## Making changes

**NOTE: Assuming you have provisioned the box with the default RAILS_ENV=development.**

The directory **/home/papyrus/papyrus** in the box is a *symlink* to **/vagrant/papyrus**, which is a synced folder in the your local machine's **vagrant-papyrus** folder.
You can make changes on your local machine in **vagrant-papyrus/papyrus** folder and it is changed in the vagrant box too. 

## Running unit tests

**NOTE: Assuming you have provisioned the box with the default RAILS_ENV=development.**

SSH into the box as user **papyrus**
```
ssh papyrus@127.0.0.1 -p2222
cd papyrus
RAILS_ENV=test bundle exec rake db:reset
RAILS_ENV=test bundle exec rake test
```

## Provisioning Papyrus on a remote server/VM

Assuming you have a remote server dedicated for running Papyrus. And suppose there is a DNS record for the server such as **papyrus.yourdomain.ca**.

The following steps will install/configure MYSQL and Papyrus on that server.

Create an **inventory** file with the name/IP address of the remote server, similar to the one below:
```
papyrus    ansible_host=192.168.168.168

[rails_app_servers]
papyrus
```

Install Mysql and Papyrus on the target server

```
ansible-playbook -i inventory papyrus_provision.yml -e"rails_env=production app_domain=yourdomain.ca mysql_root_password=db_root_password mysql_host=localhost mysql_user=papyrus mysql_password=papyrus_db_password" --limit papyrus 
```

**Don't forget to set the search API keys as explained above.**

## About Papyrus
Take a look at [Papyrus](https://github.com/yorkulibraries/papyrus) repo for Papyrus code.
