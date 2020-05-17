IPBASEWORKERS = "192.168.12."
IPMASTER = "192.168.12.10"
NOME_IMAGEM = "ubuntu/bionic64"
WORKERS = 2
MASTERS = 1

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 4096
        v.cpus = 2
    end
      
    config.vm.define "k8s-master" do |master|
        master.vm.box = NOME_IMAGEM
        master.vm.network "private_network", ip: "#{IPMASTER}"
        #master.vm.network "forwarded_port", guest: 6443, host: 6443, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 2379, host: 2380, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 10250, host: 10250, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 10251, host: 10251, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 10251, host: 10252, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 10255, host: 10255, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 30000, host: 32767, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 6781, host: 6781, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 6782, host: 6782, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 6783, host: 6783, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 6783, host: 6783, protocol: "udp"
        #master.vm.network "forwarded_port", guest: 6784, host: 6784, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 31156, host: 31156, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 32111, host: 32111, protocol: "tcp"
        #master.vm.network "forwarded_port", guest: 32222, host: 32222, protocol: "tcp"
        master.vm.hostname = "k8s-master"
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "roles/main.yml"          
            ansible.extra_vars = {
                node_ip: "#{IPMASTER}",
            }
            ansible.groups = {
                "k8s-master" => ["k8s-master-[1:#{MASTERS}]"],
                "k8s-workers" => ["k8s-workers-[1:#{WORKERS}]"]
            }   
        end
    end

    (1..WORKERS).each do |i|
        config.vm.define "k8s-workers-#{i}" do |node|
            node.vm.box = NOME_IMAGEM
            node.vm.network "private_network", ip: "#{IPBASEWORKERS}#{i + 10}"
            #node.vm.network "forwarded_port", guest: 10250, host: "1997#{i}", protocol: "tcp"
            #node.vm.network "forwarded_port", guest: 10255, host: "1998#{i}", protocol: "tcp"
            #node.vm.network "forwarded_port", guest: 30000, host: "1999#{i}", protocol: "tcp"
            node.vm.hostname = "k8s-workers-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "roles/main.yml"
                ansible.extra_vars = {
                    node_ip: "#{IPBASEWORKERS}#{i + 10}",
                }
                ansible.groups = {
                    "k8s-master" => ["k8s-master-[1:#{MASTERS}]"],
                    "k8s-workers" => ["k8s-workers-[1:#{WORKERS}]"]
                }
            end
        end
    end
end