# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/xenial64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    # Customize the amount of memory on the VM:
    vb.memory = "4096"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    #
    # Building OpenCV from source
    #
    # Required build dependencies
    # ccmake .. in order to configure
    apt-get install -y build-essential cmake cmake-gui cmake-curses-gui
    # Installing Miniconda
    su vagrant -c 'wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
      /bin/bash Miniconda3-latest-Linux-x86_64.sh -b -f && \
      source ~/miniconda3/etc/profile.d/conda.sh &&
      conda init'
    # to supoort python3
    apt-get install -y python3-dev python3-numpy
    # GTK support for GUI features
    apt-get install -y libavcodec-dev libavformat-dev libavutil-dev libswscale-dev
    apt-get install -y libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev
    # to support gtk3
    apt-get install -y libgtk-3-dev
    # Optional Dependencies
    apt-get install -y ccache
    # apt-get install -y libatlas-dev libatlas-base-dev liblapacke liblapacke-dev liblapack-dev libopenblas-base libopenblas-dev
    apt-get install -y libavresample-dev libdc1394-22-dev
    # Optional Dependencies
    apt-get install -y libpng-dev libjpeg-dev libopenexr-dev libtiff-dev libwebp-dev libjasper-dev
    apt-get install -y git
    # Downloading OpenCV
    su vagrant -c 'git clone https://github.com/opencv/opencv.git && git clone https://github.com/opencv/opencv_contrib.git'
    # Building OpenCV
    su vagrant -c 'cd opencv && mkdir build && cd build && \
      cmake  -DCMAKE_BUILD_TYPE=RELEASE \
        -DOPENCV_ENABLE_NONFREE=ON \
        -DOPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
        .. && \
      make -j2 && \
      sudo make install'
    # Generating distribution archives¶
    su vagrant -c 'python -m pip install --user --upgrade setuptools wheel && \
      cd opencv && cd build && cd python_loader && \
      python setup.py sdist bdist_wheel && \
       # pip install python_loader/dist/opencv-4.2.0-py3-none-any.whl'
  SHELL
end
