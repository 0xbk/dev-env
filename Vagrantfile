Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/lunar64"
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
    apt-get update
    apt-get upgrade
    apt-get install -y ubuntu-desktop-minimal kdiff3
    snap install chromium
    snap install code --classic
  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    # Install vscode extensions
    # General extensions
    code --install-extension ginfuru.ginfuru-better-solarized-dark-theme
    code --install-extension vscodevim.vim
    # Languages
    code --install-extension ms-python.python
    code --install-extension yzhang.markdown-all-in-one
    # Formatters
    code --install-extension esbenp.prettier-vscode
    # Linters
    code --install-extension dbaeumer.vscode-eslint
    code --install-extension SonarSource.sonarlint-vscode

    # Install nvm & node
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
    . ~/.nvm/nvm.sh
    node_version=20.5.1
    nvm install $node_version
    nvm alias default $node_version
  SHELL

  config.vm.provision "docker" do |d|
    # Empty provision block causes vagrant to only install docker
  end
end
