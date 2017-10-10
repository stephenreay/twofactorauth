# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
	config.vm.box = 'koalephant/debian9-amd64'

  [:parallels, :virtualbox].each do |provider|
    config.vm.provider provider do |vm, override|
      vm.name = 'TwoFactor Auth'
    end
  end

  config.vm.hostname = 'vagrant-local.twofactorauth.org'
  config.vm.define 'TwoFactorAuth'

	config.vm.network :forwarded_port, guest: 8888, host: 8888

	config.vm.provision :shell, privileged: true, inline: <<-SETUP
		apt-get update
		apt-get install -y jekyll
    cat <<-SERVICE > /lib/systemd/system/jekyll.service
      [Unit]
      Description=Jekyll Server
      After=network.target vagrant.mount
      
      [Service]
      User=vagrant
      Group=vagrant
      WorkingDirectory=/vagrant
      ExecStart=/usr/bin/jekyll serve --verbose --watch --force_polling --host 0.0.0.0 --port 8888
      
      [Install]
      WantedBy=multi-user.target

SERVICE
    systemctl daemon-reload
    systemctl enable jekyll.service
    systemctl start jekyll.service
	SETUP

  # config.vm.provision :shell, privileged: false, run: :always, inline: <<-RUN
    # cd /vagrant
    # systemd-run --user jekyll serve --verbose --watch --host 0.0.0.0 --port 8888
  # RUN
end
