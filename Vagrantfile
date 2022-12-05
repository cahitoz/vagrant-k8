#BOX_IMAGE = "ubuntu/focal64"
BOX_IMAGE = "generic/ubuntu2204"
CPUS_MASTER_NODE = 2
CPUS_WORKER_NODE = 2
MEMORY_MASTER_NODE = 2048
MEMORY_WORKER_NODE = 2048
WORKER_NODES_COUNT = 2

Vagrant.configure("2") do |config|
  config.vm.provision "shell", path: "scripts/bootstrap.sh"

  config.vm.define "K8Master" do |master|
    master.vm.box               = BOX_IMAGE   
    master.vm.box_check_update  = false
    master.vm.host_name         = "K8Master"

    #master.vm.network "private_network", ip: "192.168.78.100"
    master.vm.network "private_network", ip: "172.16.16.100"
    master.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", MEMORY_MASTER_NODE]
      vb.customize ["modifyvm", :id, "--cpus", CPUS_MASTER_NODE]
    end

    master.vm.provision "shell", path: "scripts/k8master.sh"
  end

  (1..WORKER_NODES_COUNT).each do |i|
    config.vm.define "K8Worker#{i}" do |worker|
      worker.vm.box = BOX_IMAGE
      worker.vm.box_check_update  = false
      worker.vm.host_name = "K8Worker#{i}"
      #worker.vm.network "private_network", ip: "192.168.78.#{i + 100}"
      worker.vm.network "private_network", ip: "172.16.16.#{i + 100}"

      worker.vm.provider  "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
        vb.customize ["modifyvm", :id, "--memory", MEMORY_WORKER_NODE]
        vb.customize ["modifyvm", :id, "--cpus", CPUS_WORKER_NODE]
      end
  
      worker.vm.provision "shell", path: "scripts/k8worker.sh"
    end
  end

end
