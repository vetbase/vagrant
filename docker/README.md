# Docker
Base Vagrant box that has Docker pre-installed and pre-configured.

## Build
-   `git clone git@github.com:vetbase/vagrant`
-   `cd vagrant/docker`
-   `vagrant up --provision`

`vagrant up --provision` will start a new Vagrant box and provision the box using Ansible. **Be patient**, this step will take awhile because Ansible will update the OS, install required packages, then reboot the box.

Once Ansible has provisied the box you can export the new box using this command:  
`vagrant package --output path/to/<NAME>.box --base vagrant-docker`
