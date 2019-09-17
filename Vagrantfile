Vagrant.require_version ">= 2.1.0" # 2.1.0 minimum required for triggers

user = ENV['RH_SUBSCRIPTION_MANAGER_USER']
password = ENV['RH_SUBSCRIPTION_MANAGER_PW']
if !user or !password
  puts 'Required environment variables not found. Please set RH_SUBSCRIPTION_MANAGER_USER and RH_SUBSCRIPTION_MANAGER_PW'
  abort
end

register_script = %{
if ! subscription-manager status; then
  sudo subscription-manager register --username=#{user} --password=#{password} --auto-attach
fi
}

unregister_script = %{
if subscription-manager status; then
  sudo subscription-manager unregister
fi
}

Vagrant.configure("2") do |config|
  config.vm.box = "roboxes/rhel8"
  config.vm.box_version = "1.9.28"
  # Disable guest additions check, because at this point the VM 
  # will not be registered with RHEL via subsctiption-manager 
  # and yum install <anything> will not work.
  config.vbguest.auto_update = false

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 2
    vb.memory = "1024"
  end # virtualbox provider

  config.vm.provision "shell", inline: register_script

  config.trigger.before :destroy do |trigger|
    trigger.name = "Before Destroy trigger"
    trigger.info = "Unregistering this VM from RedHat Subscription Manager..."
    trigger.warn = "If this fails, unregister VMs manually at https://access.redhat.com/management/subscriptions"
    trigger.run_remote = {inline: unregister_script}
    trigger.on_error = :continue
  end # trigger.before :destroy
end # vagrant configure