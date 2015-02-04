# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|
  config.vm.synced_folder '.', '/vagrant', disabled: true

  require 'rbconfig'
  def os
    @os ||= (
      host_os = RbConfig::CONFIG['host_os']
      case host_os
      when /mswin|msys|mingw|cygwin|bccwin|wince|emc/
        :windows
      when /darwin|mac os/
        :macosx
      when /linux/
        :linux
      when /solaris|bsd/
        :unix
      else
        raise Error, "unknown os: #{host_os.inspect}"
      end
    )
  end

  config.vm.define "omeka" do |app|
    app.vm.hostname = "omeka"
    app.vm.box = "trusty64"
    app.vm.box_url = "https://vagrantcloud.com/ubuntu/boxes/trusty64"

    app.vm.network :private_network, ip: "10.12.14.16"

    app.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--memory", 1536]
      case os
      when /linux/
        v.customize ["modifyvm", :id, "--cpus", `grep "^processor" /proc/cpuinfo | wc -l`.chomp]
      when /macosx/
        v.customize ["modifyvm", :id, "--cpus", `sysctl -n hw.ncpu`.chomp]
      end
    end


    config.vm.provision :ansible do |ansible|
      ansible.host_key_checking = false
      ansible.playbook = "install.yml"
      ansible.inventory_path = "inventory/vagrant"
      ansible.limit = 'all'
    end

  end
end
