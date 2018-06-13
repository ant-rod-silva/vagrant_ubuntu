# ubuntu18.04mate_vagrant
Vagrant File for Ubuntu 18.04

### Prerequisites

```
Internet connection
Oracle Virtualbox >= 5.2.12
Vagrant
Babun Shell (if Windows) - http://babun.github.io/
````

### Login and password

````
login: vagrant
password: vagrant
````

## Running

On Vagrantfile dir:

````
vagrant up 
vagrant provision
````

## Useful commands

To reload a VM and reload any new changes in the Vagrantfile config run

````
vagrant reload
````

When complete and want to exit, run

````
vagrant halt
````

When you are ready to start working again and starting fresh

````
vagrant up
````
