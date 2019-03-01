# Note:
#
# This config puts the boxes onto your local LAN.
# If you share your LAN with someone else,
# and that someone else is *also* doing development on Greeter,
# you probably want to change the last number in PRIVATE_NETWORK to something unique to your box
# End Note

# Note: use `vagrant plugin install vagrant-vbguest` or it won't work . . .

PRIVATE_NETWORK = "10.45.45"

Vagrant.configure("2") do |config|
    config.vm.define "db" do |db|
        db.vm.box = "centos/7"

        db.vm.network "private_network", ip: "#{PRIVATE_NETWORK}.4"
        db.vm.synced_folder ".", "/vagrant", type: ""

        db.vm.provision :shell, inline: "cd /vagrant && ./vagrant-db.pl #{PRIVATE_NETWORK}.4"
    end

    config.vm.define "web" do |web|
        web.vm.box = "centos/7"

        web.vm.network "private_network", ip: "#{PRIVATE_NETWORK}.5"
        web.vm.synced_folder ".", "/vagrant", type: ""

        web.vm.network "forwarded_port", guest: 8080, host: 18080

        web.vm.provision :shell, inline: "yum install -y curl postgresql wget"
        web.vm.provision :shell, privileged: false, inline: "echo 'export PGHOST=#{PRIVATE_NETWORK}.4' >> $HOME/.profile"
        web.vm.provision :shell, privileged: false, inline: "echo '*:*:*:vagrant:vagrdbpass' >> $HOME/.pgpass"
        web.vm.provision :shell, privileged: false, inline: "chmod 0600 $HOME/.pgpass"
        web.vm.provision :shell, privileged: false, inline: "cd /vagrant && ./INSTALL"
    end
end
