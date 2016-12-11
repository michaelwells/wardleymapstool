# -*- mode: ruby -*-
# vi: set ft=ruby :

# This Vagrantfile is designed to get you up and running quickly and should not
# be used as an example of a production deployment. It takes many shortcuts
#
# Once running you should be able to connect to http://localhost:8080

$install_script = <<SCRIPT
yum install -y epel-release
curl -so /etc/yum.repos.d/mongodb.repo https://repo.mongodb.org/yum/redhat/mongodb-org-3.0.repo
yum install -y mongodb-org
systemctl start mongod
chkconfig mongod on
yum install -y nodejs
yum install -y gcc-c++
cd /vagrant && npm install --production
echo "[Unit]
Description=Wardley Maps Tool
After=network.target
Wants=mongod.service

[Service]
User=vagrant
WorkingDirectory=/vagrant/
ExecStart=/usr/bin/npm start

[Install]
WantedBy=multi-user.target" > /etc/systemd/system/wardleymaps.service
systemctl enable wardleymaps
systemctl start wardleymaps
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.box = 'bento/centos-7.2'
  config.vm.provision 'shell', inline: $install_script
  config.vm.network "forwarded_port", guest: 8080, host: 8080

  # Workaround bug in some versions of Virtualbox (https://github.com/chef/bento/issues/688)
  config.vm.provider 'virtualbox' do |vb|
    vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
  end

end
