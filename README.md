- š Hi, Iām @Raquel360
- š Iām interested in ...
- š± Iām currently learning ...
- šļø Iām looking to collaborate on ...
- š« How to reach me ...

<!---
Raquel360/Raquel360 is a āØ special āØ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
$script_mysql = <<-SCRIPT
  apt-get update && \
  apt-get install -y mysql-server-5.7 && \
  mysql < /vagrant/mysql/script/user.sql && \
  mysql < /vagrant/mysql/script/schema.sql && \
  mysql < /vagrant/mysql/script/data.sql && \
  cat /vagrant/mysql/mysqld.cnf > /etc/mysql/mysql.conf.d/mysqld.cnf && \
  service mysql restart
SCRIPT

Vagrant.configure("2") do |config|

  config.vm.box = "hashicorp/bionic64"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = 1
  end 

  config.vm.define "mysqlserver" do |mysqlserver|
    mysqlserver.vm.network "forwarded_port", guest: 3306, host: 3306
    mysqlserver.vm.network "private_network", ip: "192.168.33.10"

    mysqlserver.vm.provider "virtualbox" do |vb|
      vb.name = "mysqlserver"
    end

    mysqlserver.vm.provision "shell", inline: $script_mysql
  end

  # config.vm.box_check_update = true

  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # config.vm.network "private_network", ip: "192.168.33.10"

  # config.vm.network "public_network"

end
