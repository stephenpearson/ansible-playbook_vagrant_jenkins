# Vagrant Jenkins

This repo contains a Vagrantfile suitable for building a Jenkins server.
An Ansible provisioner is used to actually deploy the Jenkins package
and configure it.

## Prequisites

Vagrant is the recommended way to build an image, so install that if you need
it.

You will need a recent (at least version 2.0) release of Ansible installed.  The
recommended way to install it is to use a virtualenv:

```
virtualenv venv
source ./venv/bin/activate
pip install ansible
```


## Steps

1. Run `vagrant up`
2. Get the admin password from the root fs
```
vagrant ssh
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
Username is admin, password is from step 2 above.
3. Log into the Jenkins server
```
# To get the IP address
vagrant ssh-config
# Then point browser to http://<address>:8080
```
4. Set Jenkins global environment variables
  * Log into Jenkins as the admin
  * Click on "Manage Jenkins" then "Configure System"
  * Scroll down to the "Global Properties" section.
  * Make sure that "Environment variables" is ticked
  * Add a `HOS_LCM_NODE` variable to point to the HOS lifecycle manager
  * Example: HOS_LCM_NODE  =>  10.20.30.10
  * Add a `JJB_GIT_URL` variable to define the GIT URL where JJB is kept.
  * Example: `JJB_GIT_URL`  =>  http://gitlab/jobs/jenkins1.git
5. Run the jenkins_job_builder_update job to update the jobs

## Interaction with HOS lifecycle manager

If you want to run jobs that interact with the Lifecycle Manager, you will need
to generate an SSH key inside the Jenkins user and copy the public key to the
stack user's authorized_keys file.

```
vagrant ssh
sudo su - jenkins
# Just use the defaults, no keypair password, to generate an SSH
# keypair (keep hitting enter)
ssh-keygen
# Copy the key to the HOS lifecycle manager ~stack user into authorized_keys
```