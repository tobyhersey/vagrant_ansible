private_key_path = File.join(Dir.home, ".ssh", "id_rsa")
public_key_path = File.join(Dir.home, ".ssh", "id_rsa.pub")
insecure_key_path = File.join(Dir.home, ".vagrant.d", "insecure_private_key")

private_key = IO.read(private_key_path)
public_key = IO.read(public_key_path)

Vagrant.configure("2")  do |config|

  config.vm.define "featuremigration" do |featuremigration|
    featuremigration.vm.box = "bento/ubuntu-18.04"
    featuremigration.vm.hostname = "featuremigration"
    featuremigration.ssh.forward_agent = true
    featuremigration.vm.network :private_network, ip: "192.168.33.10"
    featuremigration.ssh.insert_key = false
    featuremigration.ssh.private_key_path = [
        private_key_path,
        insecure_key_path # to provision the first time
    ]

    featuremigration.vm.provision :shell, :inline => <<-SCRIPT
        set -e

        echo '#{private_key}' > /home/vagrant/.ssh/id_rsa
        chmod 600 /home/vagrant/.ssh/id_rsa

        echo '#{public_key}' > /home/vagrant/.ssh/authorized_keys
        chmod 600 /home/vagrant/.ssh/authorized_keys
    SCRIPT
  end

  config.vm.define "mastermigration" do |mastermigration|
    mastermigration.vm.box = "ubuntu/trusty64"
    mastermigration.vm.hostname = "mastermigration"
    mastermigration.ssh.forward_agent = true
    mastermigration.vm.network :private_network, ip: "192.168.33.20"
    mastermigration.ssh.insert_key = false
    mastermigration.ssh.private_key_path = [
        private_key_path,
        insecure_key_path # to provision the first time
    ]

    mastermigration.vm.provision :shell, :inline => <<-SCRIPT
        set -e

        echo '#{private_key}' > /home/vagrant/.ssh/id_rsa
        chmod 600 /home/vagrant/.ssh/id_rsa

        echo '#{public_key}' > /home/vagrant/.ssh/authorized_keys
        chmod 600 /home/vagrant/.ssh/authorized_keys
    SCRIPT
    end
end

