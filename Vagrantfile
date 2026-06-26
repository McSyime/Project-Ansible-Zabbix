Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 1
  end

  # Public Key RaspberryPi
  PI_PUBLIC_KEY = "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDa6Im7E0YIx4nmQoXaxvrYbdLfdQZcUIVaYccIowqhg maxime-corthesy@MaxPi"

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

        # Configuration du hostname
        hostnamectl set-hostname #{node[:name]}

        # Installation de Tailscale
        curl -fsSL https://tailscale.com/install.sh | sh

        # Activation du service SSH
        systemctl enable ssh
        systemctl start ssh

        # Ajout de la clé publique SSH du Pi pour l'utilisateur vagrant
        mkdir -p /home/vagrant/.ssh
        echo "#{PI_PUBLIC_KEY}" >> /home/vagrant/.ssh/authorized_keys

        chown -R vagrant:vagrant /home/vagrant/.ssh
        chmod 700 /home/vagrant/.ssh
        chmod 600 /home/vagrant/.ssh/authorized_keys
      SHELL
    end
  end
end
