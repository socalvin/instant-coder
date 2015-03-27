Vagrant.configure("2") do |config|
	config.vm.box_url = "https://s3-us-west-2.amazonaws.com/figs-engineering/xubuntu-14.04.2.box"
	#config.vm.box_url = "file://./package.box"
	config.vm.hostname = "xubuntu-vm"
	config.vm.box = "xubuntu-14.04.2-box"
	config.vm.provider "virtualbox" do |v|
		v.memory = 2048
		v.cpus = 8
	end	
	config.vm.network "private_network", ip: "192.168.50.10"	
	config.vm.provider :virtualbox do |vb|
		vb.gui = true
	end
	config.vm.provision :chef_solo do |chef|
		chef.json = {
			coder: {
				user: 'dev',
				group: 'dev'
			},
			rbenv: {
				user_installs: [{ user: 'dev'}]
			},
			mysql: {
				server_root_password: 'mysqlforyou'
			},
			ssh_keys: {
				public_key: IO.read(File.expand_path("~/.ssh/id_rsa.pub")),
				private_key: IO.read(File.expand_path("~/.ssh/id_rsa"))
			},
			java: {
				install_flavor: "oracle",
				jdk_version: "7",
				oracle: {
					accept_oracle_download_terms: true
				}
			}
		}
		chef.add_recipe 'chef-instant-coder'
	end
end
