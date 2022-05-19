Setup a Papyrus instance quickly with Vagrant and Ansible. The Ansible playbooks can also be used to deploy to a real production server in a similar fashion.


## Getting started

```
git clone https://github.com/yorkulibraries/vagrant-papyrus.git
cd vagrant-papyrus
git clone https://github.com/yorkulibraries/ansible-rails.git
vagrant up
```
Watch for any error/failed tasks. If all is good then the instance is ready to use for testing.

Apache auth_basic is used for Basic Authentication. The default administrator username/password is:

```
admin/papyrus
```

Edit /etc/hosts and add an entry like followed so you can access the app from a browser at http://papyrus.library.yorku.ca/

```
192.168.168.168 papyrus.library.yorku.ca
```
