
$install_docker_script = <<SCRIPT
echo "192.168.99.105 box box" >> /etc/hosts
echo Installing Docker...
sudo systemctl stop firewalld
dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
dnf install -y java-17-openjdk git docker-ce docker-ce-cli containerd.io
systemctl enable --now docker
systemctl start docker
sudo usermod -aG docker vagrant
SCRIPT
$install_jenkins_script = <<SCRIPT
wget https://pkg.jenkins.io/redhat/jenkins.repo -O /etc/yum.repos.d/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
dnf update
dnf install -y java-17-openjdk jenkins git
systemctl enable --now jenkins
cat /var/lib/jenkins/secrets/initialAdminPassword
SCRIPT
# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure(2) do |config|
    config.ssh.insert_key = false
    config.vm.define "box" do |box|
        box.vm.box="shekeriev/centos-stream-9"
        box.vm.hostname = "box"
        box.vm.network "private_network", ip: "192.168.199.105"
        box.vm.network "forwarded_port", guest: 8080, host: 8080, auto_correct: true
        box.vm.synced_folder ".", "/vagrant"
        box.vm.provision "shell", inline: $install_docker_script, privileged: true
        box.vm.provision "shell", inline: $install_jenkins_script, privileged: true
        box.vm.provider :virtualbox do |vb|
      end
    end
end
