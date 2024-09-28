# Base Image
# VMware 지원 이미지 설정
BOX_IMAGE = "generic/ubuntu2004"
BOX_VERSION = "3.0.36"

# max number of worker nodes
N = 2

Vagrant.configure("2") do |config|
  #-----Manager Node
  config.vm.define "k8s-m" do |subconfig|
    subconfig.vm.box = BOX_IMAGE
    subconfig.vm.box_version = BOX_VERSION
    subconfig.vm.provider "vmware_desktop" do |v|
      v.memory = 2048
      v.cpus = 2
      v.linked_clone = true
    end
    subconfig.vm.hostname = "k8s-m"
    subconfig.vm.synced_folder "./", "/vagrant", disabled: true
    subconfig.vm.network "private_network", ip: "192.168.100.10"
    subconfig.vm.network "forwarded_port", guest: 22, host: 50010, auto_correct: true, id: "ssh"
    subconfig.vm.provision "shell", path: "https://raw.githubusercontent.com/jc3wrld999/k8s/refs/heads/master/init_cfg.sh", args: N
    subconfig.vm.provision "shell", path: "https://raw.githubusercontent.com/jc3wrld999/k8s/refs/heads/master/master.sh" 
  end

  #-----Worker Node
  (1..N).each do |i|
    config.vm.define "k8s-w#{i}" do |subconfig|
      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.box_version = BOX_VERSION
      subconfig.vm.provider "vmware_desktop" do |v|
        v.memory = 1536 
        v.cpus = 2
        v.linked_clone = true
      end
      subconfig.vm.hostname = "k8s-w#{i}"
      subconfig.vm.synced_folder "./", "/vagrant", disabled: true
      subconfig.vm.network "private_network", ip: "192.168.100.10#{i}"
      subconfig.vm.network "forwarded_port", guest: 22, host: "5001#{i}", auto_correct: true, id: "ssh"
      subconfig.vm.provision "shell", path: "https://raw.githubusercontent.com/jc3wrld999/k8s/refs/heads/master/init_cfg.sh", args: N
      subconfig.vm.provision "shell", path: "https://raw.githubusercontent.com/jc3wrld999/k8s/refs/heads/master/worker.sh"
    end
  end
end
