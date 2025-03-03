Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"
  config.vm.box_architecture = "amd64"
  config.vm.box_version = "202502.21.0"
  config.vm.define "dev-env"
  config.vm.synced_folder "./shared", "/vagrant", create: true
  config.ssh.insert_key = false

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "12288"
    vb.cpus = "4"
    vb.customize ["modifyvm", :id, "--vram", "64"]
    vb.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
    vb.customize ["modifyvm", :id, "--clipboard-mode", "bidirectional"]
    vb.customize ["modifyvm", :id, "--drag-and-drop", "bidirectional"]
  end

  config.vm.provision "shell", privileged: true, inline: <<-SHELL
    # Configure insecure vagrant ssh key needed for base boxes
    sshDir=/home/vagrant/.ssh
    [ -d $sshDir ] || mkdir $sshDir
    curl -Ls https://raw.githubusercontent.com/hashicorp/vagrant/main/keys/vagrant.pub >> $sshDir/authorized_keys
    chmod 700 $sshDir
    chmod 600 $sshDir/authorized_keys
    chown -R vagrant:vagrant $sshDir

    # Install software & packages
    apt-get remove -y firefox
    apt-get update
    apt-get upgrade
    apt-get install -y kdiff3 ubuntu-desktop-minimal
    apt-get autoremove
    snap install code --classic

    chrome=google-chrome-stable_current_amd64.deb
    wget https://dl.google.com/linux/direct/$chrome
    dpkg -i $chrome
    rm $chrome
  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    # Install nvm & node
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
    . ~/.nvm/nvm.sh
    node_version=22.14.0
    nvm install $node_version
    nvm alias default $node_version
  SHELL

  config.vm.provision "docker" do |d|
    # Empty provision block causes vagrant to only install docker
  end
end
