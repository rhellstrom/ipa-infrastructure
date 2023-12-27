# Vagrantfile

Vagrant.configure("2") do |config|
    # General settings
    config.vm.box = "rockylinux/9"
    config.vm.synced_folder ".", "/vagrant", disabled: true


    # FreeIPA 
    config.vm.define "ipa-server" do |ipa|
      ipa.vm.hostname = "ipa-server.ipa.test"
      ipa.vm.network "private_network", ip: "192.168.60.4"

      ipa.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
      end
    end

    # # DNS with BIND
    # config.vm.define "dns-server" do |dns|
    #   dns.vm.hostname = "dns-server.ipa.test"
    #   dns.vm.network "private_network", ip: "192.168.60.5"

    #   dns.vm.provider "virtualbox" do |vb|
    #     vb.memory = 1024
    #     vb.cpus = 1
    #   end
    # end
  
    # File Server with NFS
    config.vm.define "file-server" do |file|
      file.vm.hostname = "file-server.ipa.test"
      file.vm.network "private_network", ip: "192.168.60.6"

      file.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
      end
    end
  
    # # Web Server with HTTPD
    # config.vm.define "web-server" do |web|
    #     web.vm.hostname = "web-server.ipa.test"
    #     web.vm.network "private_network", ip: "192.168.60.7"

    #     web.vm.provider "virtualbox" do |vb|
    #     vb.memory = 1024
    #     vb.cpus = 1
    #     end
    # end

    # Simple host for testing
    config.vm.define "test-user" do |user|
      user.vm.hostname = "test-user.ipa.test"
      user.vm.network "private_network", ip: "192.168.60.8"

      user.vm.provider "virtualbox" do |vb|
        vb.memory = 2048
        vb.cpus = 2
      end
    end
    

end