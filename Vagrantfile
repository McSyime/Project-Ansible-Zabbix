Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 1
  end

  nodes = [
    { name: "vm1", ip: "192.168.56.21" },
    { name: "vm2", ip: "192.168.56.22" },
    { name: "vm3", ip: "192.168.56.23" }
  ]

  nodes.each do |node|
    config.vm.define node[:name] do |machine|
      machine.vm.hostname = node[:name]
      machine.vm.network "private_network", ip: node[:ip]

      machine.vm.provision "shell", inline: <<-SHELL
        apt update
        apt install -y curl openssh-server

        curl -fsSL https://tailscale.com/install.sh | sh

        systemctl enable ssh
        systemctl start ssh
      SHELL
    end
  end
end