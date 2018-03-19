BOX_NAME="alicankustemur/k-generic-box"
BOX_VERSION="18.03.16"
PRIVATE_IP="172.27.44.10"

Vagrant.configure("2") do |config|
    config.vm.box = "#{BOX_NAME}"
    config.vm.box_version = "#{BOX_VERSION}"
    config.vm.provision "shell", inline: "docker run --name nginx -p 80:80 -d nginx:1.12"
    
    config.vm.define "nginx" do |node|
        node.vm.hostname = "nginx"
        node.vm.network "private_network", ip: "#{PRIVATE_IP}"
        node.vm.provider "virtualbox" do |vb|
          vb.memory = "1024"
          vb.cpus = 1
        end
        node.vm.provider :aws do |aws, override|
            
            aws.tags = {
                'Name' => 'nginx'
            }
            aws.access_key_id = ENV["AWS_ACCESS_KEY_ID"]
            aws.secret_access_key = ENV["AWS_SECRET_ACCESS_KEY"]
            aws.region = ENV["AWS_DEFAULT_REGION"]
            aws.keypair_name = ENV["AWS_KEYPAIR_NAME"]
            aws.instance_type="t2.nano"
            aws.subnet_id = ENV["AWS_SUBNET_ID"]
            aws.associate_public_ip = true
            aws.security_groups = ENV["AWS_SECURITY_GROUPS"]
            aws.private_ip_address = "#{PRIVATE_IP}" 
            
                override.ssh.username = "ubuntu"
                override.ssh.private_key_path = ENV["AWS_SSH_PRIVATE_KEY_PATH"]
                override.nfs.functional = false
        end
    end
end
