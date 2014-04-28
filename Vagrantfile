require 'yaml'

dir = File.dirname(File.expand_path(__FILE__))

configValues = YAML.load_file("#{dir}/puphpet/config.yaml")
data = configValues['vagrantfile-local']

if !data['vm']['provider']['virtualbox'].empty?
  ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'
end

Vagrant.configure("2") do |config|
  config.vm.box = "#{data['vm']['box']}"
  config.vm.box_url = "#{data['vm']['box_url']}"

  if data['vm']['hostname'].to_s != ''
    config.vm.hostname = "#{data['vm']['hostname']}"
  end

  if data['vm']['network']['private_network'].to_s != ''
    config.vm.network "private_network", ip: "#{data['vm']['network']['private_network']}"
  end

  data['vm']['network']['forwarded_port'].each do |i, port|
    if port['guest'] != '' && port['host'] != ''
      config.vm.network :forwarded_port, guest: port['guest'].to_i, host: port['host'].to_i
    end
  end

  data['vm']['synced_folder'].each do |i, folder|
    if folder['source'] != '' && folder['target'] != '' && folder['id'] != '' && folder['sync_type'] != ''
      if folder['sync_type'] == 'nfs'
        config.vm.synced_folder "#{folder['source']}", "#{folder['target']}", id: "#{folder['id']}", type: "nfs"
      elsif folder['sync_type'] == 'smb'
        config.vm.synced_folder "#{folder['source']}", "#{folder['target']}", id: "#{folder['id']}", type: "smb"
      elsif folder['sync_type'] == 'rsync'
        rsync_args = !folder['rsync']['args'].nil? ? folder['rsync']['args'] : ["--verbose", "--archive", "--delete", "-z"]
        rsync_auto = !folder['rsync']['auto'].nil? ? folder['rsync']['auto'] : true
        rsync_exclude = !folder['rsync']['exclude'].nil? ? folder['rsync']['exclude'] : [".vagrant/"]

        config.vm.synced_folder "#{folder['source']}", "#{folder['target']}",
        id: "#{folder['id']}",
        rsync__args: rsync_args,
        rsync__exclude: rsync_exclude,
        rsync__auto: rsync_auto,
        type: "rsync"
      else
        config.vm.synced_folder "#{folder['source']}", "#{folder['target']}", id: "#{folder['id']}"
      end
    end
  end

  config.vm.usable_port_range = (10200..10500)

  if !data['vm']['provider']['virtualbox'].empty?
    config.vm.provider :virtualbox do |virtualbox|
      data['vm']['provider']['virtualbox']['modifyvm'].each do |key, value|
        if key == "natdnshostresolver1"
          value = value ? "on" : "off"
        end
        virtualbox.customize ["modifyvm", :id, "--#{key}", "#{value}"]
      end
    end
  end

  config.vm.provision "shell" do |s|
    s.path = "puphpet/shell/initial-setup.sh"
    s.args = "/vagrant/puphpet"
  end
  config.vm.provision :shell, :path => "puphpet/shell/update-puppet.sh"
  config.vm.provision :shell, :path => "puphpet/shell/r10k.sh"

  config.vm.provision :puppet, :options => ['--debug', '--evaltrace', '--verbose'] do |puppet|
    ssh_username = !data['ssh']['username'].nil? ? data['ssh']['username'] : "vagrant"
    puppet.facter = {
      "ssh_username"     => "#{ssh_username}",
      "provisioner_type" => ENV['VAGRANT_DEFAULT_PROVIDER'],
      "vm_target_key"    => 'vagrantfile-local',
    }
    puppet.manifests_path = "#{data['vm']['provision']['puppet']['manifests_path']}"
    puppet.manifest_file = "#{data['vm']['provision']['puppet']['manifest_file']}"

    if !data['vm']['provision']['puppet']['options'].empty?
      puppet.options = data['vm']['provision']['puppet']['options']
    end
  end

  if File.file?("#{dir}/puphpet/files/dot/ssh/id_rsa")
    config.ssh.private_key_path = [
      "#{dir}/puphpet/files/dot/ssh/id_rsa",
      "#{dir}/puphpet/files/dot/ssh/insecure_private_key"
    ]
  end

  ssh_username = !data['ssh']['username'].nil? ? data['ssh']['username'] : "vagrant"

  config.vm.provision "shell" do |kg|
    kg.path = "puphpet/shell/ssh-keygen.sh"
    kg.args = "#{ssh_username}"
  end

  config.vm.provision :shell, :path => "puphpet/shell/execute-files.sh"

  if !data['ssh']['host'].nil?
    config.ssh.host = "#{data['ssh']['host']}"
  end
  if !data['ssh']['port'].nil?
    config.ssh.port = "#{data['ssh']['port']}"
  end
  if !data['ssh']['username'].nil?
    config.ssh.username = "#{data['ssh']['username']}"
  end
  if !data['ssh']['guest_port'].nil?
    config.ssh.guest_port = data['ssh']['guest_port']
  end
  if !data['ssh']['shell'].nil?
    config.ssh.shell = "#{data['ssh']['shell']}"
  end
  if !data['ssh']['keep_alive'].nil?
    config.ssh.keep_alive = data['ssh']['keep_alive']
  end
  if !data['ssh']['forward_agent'].nil?
    config.ssh.forward_agent = data['ssh']['forward_agent']
  end
  if !data['ssh']['forward_x11'].nil?
    config.ssh.forward_x11 = data['ssh']['forward_x11']
  end
  if !data['vagrant']['host'].nil?
    config.vagrant.host = data['vagrant']['host'].gsub(":", "").intern
  end

end

