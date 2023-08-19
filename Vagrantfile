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
  config.vm.box = "ubuntu/jammy64"

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
  config.vm.synced_folder "./ros_ws", "/home/vagrant/ros_ws"

  # Disable the default share of the current code directory. Doing this
  # provides improved isolation between the vagrant box and your host
  # by making sure your Vagrantfile isn't accessable to the vagrant box.
  # If you use this you may want to enable additional shared subfolders as
  # shown above.
  # config.vm.synced_folder ".", "/vagrant", disabled: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = true
  
    # Customize the amount of memory on the VM:
    vb.memory = "16384"
    vb.cpus = "8"
    
    vb.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
    vb.customize ["modifyvm", :id, "--vram", "128"]
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    echo "==========================================\n"
    echo "1. Initial OS + Ubuntu Desktop Setup\n"
    echo "==========================================\n\n"

    apt-get update
    apt-get install -y ubuntu-desktop
    apt-get install -y virtualbox-guest-dkms virtualbox-guest-utils virtualbox-guest-x11

    echo "\n\n"
    echo "==========================================\n"
    echo "2. Installing ROS Humble\n"
    echo "==========================================\n\n"

    echo "\n\n"
    echo "-------------\n"
    echo "2.1 - Locale Setup\n"
    echo "-------------\n\n"

    apt update
    apt install locales
    locale-gen en_US en_US.UTF-8
    update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
    export LANG=en_US.UTF-8

    echo "\n\n"
    echo "-------------\n"
    echo "2.2 - Add Additional Repositories\n"
    echo "-------------\n\n"

    apt install -y software-properties-common
    add-apt-repository universe
    apt update
    apt install -y curl
    curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
    # Finally, refresh the package lists:
    apt update
    apt upgrade # Recommended as ROS2 is tied tightly to ubuntu releases apparently

    echo "\n\n"
    echo "-------------\n"
    echo "2.3 - Install ROS2\n"
    echo "-------------\n\n"

    apt install -y ros-humble-desktop # Includes rviz + demos
    apt install -y python3-colcon-common-extensions

    echo "\n\n"
    echo "-------------\n"
    echo "2.4 - Source ROS\n"
    echo "-------------\n\n"

    echo 'source /opt/ros/humble/setup.bash' >> /home/vagrant/.bashrc

    echo "\n\n"
    echo "==========================================\n"
    echo "3. Installing Navigation2\n"
    echo "==========================================\n\n"

    echo "\n\n"
    echo "-------------\n"
    echo "3 - Install Navigation 2\n"
    echo "-------------\n\n"

    apt install -y ros-humble-navigation2
    apt install -y ros-humble-nav2-bringup

    echo "\n\n"
    echo "-------------\n"
    echo "Addenum - Install Cyclone DDS\n"
    echo "-------------\n\n"
    apt install -y ros-humble-rmw-cyclonedds-cpp
    echo 'export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp' >> /home/vagrant/.bashrc

    apt install -y ros-humble-slam-toolbox
    apt install -y ros-humble-tf-transformations

    echo "\n\n"echo "\n\n"
    echo "==========================================\n"
    echo "4. Installing Turtlebot3\n"
    echo "==========================================\n\n"

    echo 'export TURTLEBOT3_MODEL=waffle' >> /home/vagrant/.bashrc
    apt install -y ros-humble-turtlebot3*

    echo "\n\n"
    echo "==========================================\n"
    echo "5. Installing Cartographer\n"
    echo "==========================================\n\n"
    
    apt install -y ros-humble-cartographer-ros


    echo "\n\n"echo "\n\n"
    echo "==========================================\n"
    echo "6. Installing other tools\n"
    echo "==========================================\n\n"

    echo "\n\n"
    echo "-------------\n"
    echo "6.1 - Install colcon\n"
    echo "-------------\n\n"

    apt install -y python3-colcon-common-extensions
    apt install -y python3-colcon-clean

    echo "\n\n"
    echo "-------------\n"
    echo "6.2 - Install VSCode\n"
    echo "-------------\n\n"
    
    curl -Lk 'https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-x64' --output vscode_cli.tar.gz
    tar -xf vscode_cli.tar.gz
    mv code /usr/local/bin/


  SHELL
end
