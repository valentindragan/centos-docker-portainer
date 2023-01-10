# created by Valentin Dragan, Nov 2021
### configuration parameters ###
VM_NAME = "CDP1"
VM_OS = "geerlingguy/centos7"
VM_IP = "192.168.55.31"
VM_AUTOMATIC_UPDATES = true
VM_RAM = "2048"
VM_CPU = "1"
VM_PROVIDER = "virtualbox"
#VM_PROVIDER = "vmware_desktop"

# Automatically installs required plugin on Windows
if Vagrant::Util::Platform.windows?
    #define plugins
    plugin = 'vagrant-winnfsd'
    # install plugins
    system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin?(plugin)
end

Vagrant.configure(2) do |config|
    config.vm.box = VM_OS
    config.vm.hostname = VM_NAME
    #config.vm.network "public_network"
    config.vm.network "private_network", ip: VM_IP
    # Automatic updates
    config.vm.box_check_update = VM_AUTOMATIC_UPDATES
    # set open ports  
	
	# Sync shared directories
    config.vm.synced_folder "www", "/var/www/html", type: "nfs", linux__nfs_options: ['rw','no_subtree_check','all_squash','async']

    # ORACLE VIRTUALBOX and hardware config
    config.vm.provider VM_PROVIDER do |v|
        v.name = VM_NAME # VM name
        v.memory = VM_RAM # RAM
        v.cpus = VM_CPU # CPU count
		#v.vmx["memsize"] = VM_RAM # RAM - vmware
		#v.vmx["numvcpus"] = VM_CPU - vmware
        v.customize ["modifyvm", :id, "--vram", "64"] # video ram memory
        v.customize ["modifyvm", :id, "--clipboard",   "bidirectional"] # copy/paste functionality
        v.customize ["modifyvm", :id, "--draganddrop", "bidirectional"] # draganddrop functionality
        v.customize ["modifyvm", :id, "--vrde", "off"] # disable remote desktop
    end
	
	# VMWARE CONFIG
    # ORACLE VIRTUALBOX and hardware config
    # config.vm.provider VM_PROVIDER do |v|      
		#v.vmx["memsize"] = VM_RAM # RAM - vmware
		#v.vmx["numvcpus"] = VM_CPU - vmware      
    #end
	
	
	#Run Ansible files
    config.vm.provision "ansible_local" do |ansible|
        ansible.verbose = "vv"
        ansible.become = true # execute as root
        ansible.playbook = "ansible/playbook.yml"
    end


end