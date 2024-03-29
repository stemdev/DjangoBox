Vagrant.configure("2") do |config|

  # Ubuntu 13.04
  config.vm.box = "raring64"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/raring/current/raring-server-cloudimg-amd64-vagrant-disk1.box"
 
  # Allocate host resources
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ["modifyvm", :id, "--cpus", "4"] 
  end

  # Network
  config.vm.network "forwarded_port", guest: 80, host: 8008

  # Vagrant Cachier
  # vagrant plugin install vagrant-cachier
  if defined? config.cache
      config.cache.auto_detect = true
  end

  config.vm.provision :shell, :inline => $startup


end

$startup = <<EOF
  
  # Shared Directory
  curUser=$(whoami)
  if [[ $curUser=="vagrant" ]]
  then
      cd /vagrant
      echo "cd /vagrant" | sudo tee -a ~vagrant/.profile
  fi

  # OS
  sudo apt-get update
 
  # Tools
  sudo apt-get -y install build-essential # g++, make, etc.
  sudo apt-get -y install git

  # Django
  sudo apt-get -y install libsqlite3-dev sqlite3 bzip2 libbz2-dev #dependencies

  wget https://bitbucket.org/pypa/setuptools/raw/bootstrap/ez_setup.py
  sudo python ez_setup.py
  rm setuptools*
  rm ez_setup.py

  git clone https://github.com/django/django.git
  cd django
  sudo python setup.py install
  cd ..
  rm -rf django



EOF
