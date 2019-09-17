# Vagrantfile for RHEL 8

Vagrantfile to spin up a RHEL 8 VM and register with RHN via subscription-manager. 

# Requirements

To register with subscription-manager a free RedHat Developer subscription is required. This Vagrantfile expects to find the credentials in environment variables called `RH_SUBSCRIPTION_MANAGER_USER` and `RH_SUBSCRIPTION_MANAGER_PW`. Ensure these are exported and available to Vagrant, the Vagrantfile will abort if these are not set. 

# Notes

Using the current latest versions of Vagrant and VirtualBox on MacOS, the version of VirtualBox Guest Additions is newer than the version packaged in roboxes/rhel8. Vagrant will try and update this before the VM has been registered with RHN so all calls to yum install fail. For this reason `config.vbguest.auto_update = false` is configured.

