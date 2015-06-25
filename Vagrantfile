Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"

  config.vm.network "private_network", ip: "10.10.10.11"

 # config.vm.synced_folder "./webroot", "/webroot"

  config.vm.provider "virtualbox" do |vb|

  #   vb.gui = true

      vb.memory = "1024"
  end
  
  config.vm.post_up_message = "Modsecurity test environment. To get started visit http://10.10.10.11"

  # Spin up Apache2 with modsecurity
  # and enable sample rules
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get install -y apache2
    sudo apt-get install -y libapache2-modsecurity
    cp /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf
    sed -i 's/DetectionOnly/On/' /etc/modsecurity/modsecurity.conf
    echo 'Include "/usr/share/modsecurity-crs/*.conf"' >> /etc/modsecurity/modsecurity.conf
    echo 'Include "/usr/share/modsecurity-crs/activated_rules/*.conf"' >> /etc/modsecurity/modsecurity.conf
    ln -s /usr/share/modsecurity-crs/base_rules/modsecurity_crs_41_sql_injection_attacks.conf /usr/share/modsecurity-crs/activated_rules/
    ln -s /usr/share/modsecurity-crs/base_rules/modsecurity_crs_41_xss_attacks.conf /usr/share/modsecurity-crs/activated_rules/
    sudo a2enmod security2
    sudo /etc/init.d/apache2 force-reload
  SHELL
end
