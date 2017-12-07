
VAGRANTFILE_API_VERSION = '2'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Base VM OS configuration.
  config.vm.box = "centos/7"
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.vm.hostname = "consul"

  config.vm.provision :shell do |shell|
    shell.inline = "sudo yum install -y epel-release; sudo yum update -y"
  end

  N = 2
  (1..N).each do |machine_id|
    config.vm.define "consul-#{machine_id}" do |machine|
      machine.vm.hostname = "consul-#{machine_id}"
      machine.vm.network :private_network, ip: "192.168.23.#{10+machine_id}"

      # Virtualbox configuration
      config.vm.provider :virtualbox do |virtualbox|
        virtualbox.name = "consul-#{machine_id}"
        virtualbox.customize ['modifyvm', :id, '--memory', '2048']
        virtualbox.customize ['modifyvm', :id, '--ioapic', 'on']
        virtualbox.customize ['modifyvm', :id, '--cpus', '2']
        virtualbox.customize ['modifyvm', :id, '--cpuexecutioncap', '50']
        virtualbox.customize ['modifyvm', :id, '--groups', '/consul']
      end

      if machine_id == 1
        machine.vm.network :forwarded_port, id: 'consul_webui', guest: 8500, host: 8500
      end

      # Only execute once the Ansible provisioner,
      # when all the machines are up and ready.
      if machine_id == N
        machine.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "tests/test.yml"
          ansible.groups = {
              'consul_agents' => (1..N).map { |element| "consul-#{element}" }
          }
          ansible.host_vars = {
              'consul-1' => {
                  'consul_webui_enabled' => true
              }
          }
          ansible.extra_vars = {
              web_interface: 'eth0',
              cluster_interface: 'eth1',
              consul_join_hosts: %w(192.168.23.11 192.168.23.12)
          }
        end
      end
    end
  end

end
