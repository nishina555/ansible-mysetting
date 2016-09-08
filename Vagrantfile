Vagrant.configure(2) do |config|
	config.vm.box = "centos/7"
	config.vm.network :private_network, ip:"192.168.200.100"
	config.vm.network :forwarded_port, guest: 80, host: 8082
	config.vm.provider :virtualbox do |vbox|
		vbox.name = "ansible-test"
	end
	config.vm.provision "ansible" do |ansible|
		ansible.playbook = "site.yml"
		ansible.inventory_path = "hosts"
		ansible.limit = 'all'
	end
end
