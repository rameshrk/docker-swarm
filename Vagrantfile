servers=[
  {
    :hostname => "manager",
    :ip => "192.168.100.10",
    :box => "ubuntu/trusty64",
    :ram => 1024,
    :cpu => 2
  },
  {
    :hostname => "worker-1",
    :ip => "192.168.100.11",
    :box => "ubuntu/trusty64",
    :ram => 1024,
    :cpu => 2
  },
  {
    :hostname => "worker-2",
    :ip => "192.168.100.12",
    :box => "ubuntu/trusty64",
    :ram => 1024,
    :cpu => 2
  }
]
Vagrant.configure(2) do |config|
  servers.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box = machine[:box]
      node.vm.hostname = machine[:hostname]
      node.vm.network "private_network", ip: machine[:ip]
      if machine[:hostname] == "manager"
       #node.vm.provision "docker", images: ["monicagangwar/docker-swarm-vagrant"]
        node.vm.provision "docker" do |docker|
    	  docker.build_image "/vagrant",
      	  args: "-t example/hello_web"
        docker.run "hello_web",
          image: "example/hello_web:latest",
          args: "-p 80:80"
        end

	node.vm.network "forwarded_port", guest: 8000, host: 8000
      elsif machine[:hostname] == "worker-1"
	node.vm.provision "docker"
	node.vm.network "forwarded_port", guest: 8000, host: 8001
      elsif machine[:hostname] == "worker-2"
        node.vm.provision "docker"
	node.vm.network "forwarded_port", guest: 8000, host: 8002
      end
      node.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
      end
    end
  end
end
