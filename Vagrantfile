# -*- mode: ruby -*-
# vi: set ft=ruby :

members = {
  #  Name     #, CPU, RAM, 1ST_IP
  'etcd' => [ 3,   1,  256,     11 ],
  'node' => [ 4,   2, 2048,    100 ],
}
PREFIX   = "10.10.10"

Vagrant.configure("2") do |config|
  config.vm.box     = "coreos-alpha"
  config.vm.box_url = "http://alpha.release.core-os.net/amd64-usr/current/coreos_production_vagrant.json"

  INITIAL_CLUSTER = Array.new(members['etcd'][0]) { |i| "%s-%02d=http://%s.%02d:2380" % ['etcd', i+1, PREFIX, members['etcd'][3]+i] } * ","

  members.each do |name, (count, cpu, ram, ipstart)|
    (1..count).each do |i|
      config.vm.define member = "%s-%02d" % [name, i] do |m|
        m.vm.hostname = member
        config.vm.provider :virtualbox do |v|
          v.name                  = member
          v.cpus                  = cpu
          v.memory                = ram
          v.check_guest_additions = false
          v.functional_vboxsf     = false
        end
        ip = "%s.%02d" % [PREFIX, ipstart+i-1]
        m.vm.network :private_network, ip: ip

        if name == 'etcd' then
           m.vm.provision :file,  :source => "./etcd.yaml",   :destination => "/tmp/cloud-config.yaml"
        elsif i == 1 then
            m.vm.provision :file, :source => "./master.yaml", :destination => "/tmp/cloud-config.yaml"
        else
          m.vm.provision :file,   :source => "./node.yaml",   :destination => "/tmp/cloud-config.yaml"
        end
        m.vm.provision :shell, inline: <<-SCRIPT
              sed -i -e "s|__HOSTNAME__|#{member}|" \
                     -e "s|__INITIAL_CLUSTER__|#{INITIAL_CLUSTER}|" \
              /tmp/cloud-config.yaml
              SCRIPT

        m.vm.provision :shell, :inline => "mv /tmp/cloud-config.yaml /var/lib/coreos-vagrant/vagrantfile-user-data", :privileged => true

      end
    end
  end
end
