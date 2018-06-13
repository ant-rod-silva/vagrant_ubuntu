# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant automated VM for development
# By default, this is set to C:\Users\yourusername\.vagrant.d (Windows)

box_name = 'django'
box_memory = '2048'
box_hostname = 'django'
box_cpus = 2
box_ip = '192.168.99.100'
private_key_path = 'C:\Users\rodri\.vagrant.d\insecure_private_key'
color_off = "\033[0m"
color_red = "\033[31m"
color_yellow = "\033[33m"

Vagrant.configure("2") do |config|
  config.vm.box_download_insecure = true
  config.vm.box = "martijnsips/UbuntuMate1804LTS"
  config.vm.box_version = "1.0.0"

  # Configure virtual machine specs. Keep it simple, single user.
  config.vm.provider "virtualbox" do |v|
    v.name = "#{box_name}"
    v.memory = "#{box_memory}"
    v.cpus = "#{box_cpus}"
    # Setup videocard  
    v.customize ["modifyvm", :id, "--vram", "128"]  
    v.customize ["modifyvm", :id, "--accelerate3d", "on"]  

    # Setup copy/pasta  
    v.customize ["modifyvm", :id, "--clipboard", "bidirectional"]  
    v.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]  
  end
  
  # Config hostname and IP address so entry can be added to HOSTS file
  config.vm.hostname = "#{box_hostname}"
  config.vm.network :private_network, ip: "#{box_ip}"

  # Forward a port from the guest to the host, which allows for outside
  # computers to access the VM, whereas host only networking does not.
  config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 8000, host: 8000, host_ip: "127.0.0.1"

  config.vm.post_up_message = "UBUNTU 18.04 LTS INSTALLATION"
  
  # ssh settings
  if Vagrant::Util::Platform.windows? then
    # 
  else
    # other os code
  end
  config.vm.synced_folder "opt/", "/opt", create: true
 
  # Provisioning django application using shell
  # vagrant provision
  config.vm.provision :shell,
  inline: <<-SHELL
    cd /tmp

	echo -e "#{color_red}\n============= enable sudo without asking for a password for vagrant user =============#{color_off}\n"
    sudo groupadd -r admin
	sudo usermod -a -G admin vagrant
	sudo gpasswd -d vagrant sudo
    sudo sed -i -e 's,%admin ALL=(ALL) ALL,%admin ALL=(ALL) NOPASSWD:ALL,g' /etc/sudoers
	
	echo -e "#{color_red}\n============= Installing Linux Headers =============#{color_off}\n"
	sudo apt-get install -y linux-headers-$(uname -r)

	echo -e "#{color_red}\n============= Add Canonical Partners Repository =============#{color_off}\n"
    sudo add-apt-repository "deb http://archive.canonical.com/ $(lsb_release -sc) partner"	

	echo -e "#{color_red}\n============= Add Java Repository =============#{color_off}\n"
	sudo add-apt-repository ppa:linuxuprising/java
	
	echo -e "#{color_red}\n============= Add unetbootin Repository =============#{color_off}\n"
	sudo apt-add-repository ppa:gezakovacs/ppa sudo apt install unetbootin

	echo -e "#{color_red}\n============= Add gscan2pdf Repository =============#{color_off}\n"
	sudo add-apt-repository ppa:jeffreyratcliffe/ppa

	echo -e "#{color_red}\n============= Add notepadqq Repository =============#{color_off}\n"
	sudo add-apt-repository ppa:notepadqq-team/notepadqq

	echo -e "#{color_red}\n============= Add krita Repository =============#{color_off}\n"
	sudo add-apt-repository ppa:kritalime/ppa

	echo -e "#{color_red}\n============= Add Opera Repository =============#{color_off}\n"
	sudo sh -c 'echo "deb http://deb.opera.com/opera-stable/ stable non-free" >> /etc/apt/sources.list.d/opera.list'
    wget -qO- https://deb.opera.com/archive.key | sudo apt-key add -

	echo -e "#{color_red}\n============= Add Visual Studio Code Repository =============#{color_off}\n"
	curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'

	
	echo -e "#{color_red}\n============= Updating virtual machine =============#{color_off}\n"
    sudo apt update
	sudo apt install --no-install-recommends -y aptitude-common aptitude

	echo -e "#{color_red}\n============= Installing utility to install local deb package =============#{color_off}\n"
	sudo apt install --no-install-recommends -y gdebi ppa-purge

	echo -e "#{color_red}\n============= Clean / upgrade packages =============#{color_off}\n"
	# sudo apt upgrade -y
	sudo apt autoremove -y
	sudo aptitude clean
	sudo aptitude autoclean

	echo -e "#{color_red}\n============= Installing prerequisites =============#{color_off}\n"
	sudo apt install --no-install-recommends -y gconf-service gconf-service-backend gconf2 gconf2-common libgconf-2-4 screen

	echo -e "#{color_red}\n============= Installing C library =============#{color_off}\n"
	sudo apt autoremove -y
	sudo apt install --no-install-recommends -y libc6-dev g++ g++-7 cpp gcc cmake

	echo -e "#{color_red}\n============= Installing compilers =============#{color_off}\n"
	sudo apt install --no-install-recommends -y gfortran libgfortran3 fpc m4
	sudo apt install --no-install-recommends -y libx11-dev libpq-dev libldap2-dev libsasl2-dev libncurses5 libncurses5-dev
    sudo apt install --no-install-recommends -y libxml2 libxml2-dev libxslt-dev libblas-dev liblapack-dev libatlas-base-dev libjpeg8-dev libffi6
    sudo apt install --no-install-recommends -y libffi-dev libcairo2 libpango1.0-0 libpangox-1.0-0 libpangocairo-1.0-0 libgdk-pixbuf2.0-0 libssl-dev
	sudo apt install --no-install-recommends -y libyaml-dev

	echo -e "#{color_red}\n============= ruby and rails =============#{color_off}\n"
    sudo apt install --no-install-recommends -y ruby rails

	echo -e "#{color_red}\n============= Installing JDK 10 =============#{color_off}\n"
	echo "oracle-java10-installer shared/accepted-oracle-license-v1-1 select true" | sudo debconf-set-selections
	sudo apt install --no-install-recommends -y oracle-java10-installer
    sudo apt install --no-install-recommends -y oracle-java10-set-default

	echo -e "#{color_red}\n============= Installing time sync =============#{color_off}\n"
	sudo apt install --no-install-recommends -y chrony

	echo -e "#{color_red}\n============= Installing office apps =============#{color_off}\n"
	sudo apt install --no-install-recommends -y mc-data mc vim nano ispell meld gedit pdftk
	sudo apt install --no-install-recommends -y curl

	echo -e "#{color_red}\n============= Installing control version apps =============#{color_off}\n"
	sudo apt install --no-install-recommends -y git git-core gitk
	
	echo -e "#{color_red}\n============= Installing docker vagrant =============#{color_off}\n"
	sudo apt install --no-install-recommends -y ansible docker vagrant

	echo -e "#{color_red}\n============= Installing python libs =============#{color_off}\n"
	sudo apt install --no-install-recommends -y python-pip
	sudo apt install --no-install-recommends -y python-dev python2.7-dev python-all python-all-dev 
	sudo apt install --no-install-recommends -y libpython-dev libpython2.7-dev libpython-all-dev
	sudo apt install --no-install-recommends -y python-setuptools python-pip python-pip python-simplejson python-suds
	sudo apt install --no-install-recommends -y python-wheel ipython

	echo -e "#{color_red}\n============= Installing terminal apps =============#{color_off}\n"
	sudo apt install --no-install-recommends -y guake terminator
	
	echo -e "#{color_red}\n============= Installing app manager =============#{color_off}\n"
	sudo apt install --no-install-recommends -y synaptic

	echo -e "#{color_red}\n============= Installing terminal =============#{color_off}\n"
	sudo apt install --no-install-recommends -y zsh zsh-common
	
	echo -e "#{color_red}\n============= Installing virtualenv =============#{color_off}\n"
	sudo apt install --no-install-recommends -y virtualenv virtualenv-clone virtualenvwrapper python-virtualenv python3-virtualenv

	sudo apt install --no-install-recommends -y xvfb whois shared-mime-info numlockx net-tools
	sudo apt install --no-install-recommends -y openssh-server openssl libssl-dev  
	
	echo -e "#{color_red}\n============= Installing essential packages =============#{color_off}\n"
	sudo apt install --no-install-recommends -y build-essential

	echo -e "#{color_red}\n============= Installing file compression =============#{color_off}\n"
	sudo apt install --no-install-recommends -y arj p7zip p7zip-full p7zip-rar rar unrar unace unace-nonfree unzip zip sharutils
    sudo apt install --no-install-recommends -y sharutils uudeview mpack arj cabextract
	
	echo -e "#{color_red}\n============= Installing Maintenance Tools =============#{color_off}\n"
	sudo apt install --no-install-recommends -y gparted bleachbit trash-cli nmap ufw monit htop iotop iftop fail2ban	
	sudo snap easy-disk-cleaner
	
	echo -e "#{color_red}\n============= Installing remote desktop apps =============#{color_off}\n"
	sudo apt install --no-install-recommends -y xrdp rdesktop
	sudo systemctl enable xrdp

	echo -e "#{color_red}\n============= Installing Google Chrome dependencies =============#{color_off}\n"
	sudo apt install --no-install-recommends -y libappindicator1 libindicator7

	echo -e "#{color_red}\n============= Installing Browsers =============#{color_off}\n"
	sudo apt install -y chromium-browser chromium-codecs-ffmpeg-extra
	wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
	sudo gdebi google-chrome-stable_current_amd64.deb
	https://dl.google.com/linux/direct/google-talkplugin_current_amd64.deb
	sudo gdebi google-talkplugin_current_amd64.deb

	echo -e "#{color_red}\n============= Installing Flash Plugin =============#{color_off}\n"
	sudo apt install --no-install-recommends -y adobe-flashplugin browser-plugin-freshplayer-pepperflash pepperflashplugin-nonfree	
	
	echo -e "#{color_red}\n============= Installing krita =============#{color_off}\n"
    sudo apt-get install krita
	
	echo -e "#{color_red}\n============= Installing VLC =============#{color_off}\n"
	sudo snap install vlc --classic
	
	echo -e "#{color_red}\n============= Installing microsoft fonts (before ubuntu-restricted-extras, always) =============#{color_off}\n"
	mkdir -p ~/mscorefonts/files;
    cd ~/mscorefonts/;
	arrr=(andale32.exe arial32.exe arialb32.exe comic32.exe courie32.exe georgi32.exe impact32.exe times32.exe trebuc32.exe verdan32.exe webdin32.exe); for i in "${arrr[@]}"; do wget "https://netcologne.dl.sourceforge.net/project/corefonts/the%20fonts/final/$i";done
	wget http://ftp.br.debian.org/debian/pool/contrib/m/msttcorefonts/ttf-mscorefonts-installer_3.6_all.deb
	sudo gdebi ttf-mscorefonts-installer_3.6_all.deb
	echo "ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true" | sudo debconf-set-selections
    sudo apt install --no-install-recommends -y fontconfig ttf-mscorefonts-installer
	cd /tmp

	echo -e "#{color_red}\n============= Installing ubuntu fonts =============#{color_off}\n"
	sudo apt install --no-install-recommends -y ttf-ubuntu-font-family fonts-ubuntu-font-family-console
	
	echo -e "#{color_red}\n============= Installing codecs =============#{color_off}\n"
	sudo apt install --no-install-recommends -y libavcodec-extra57 libavcodec-extra ffmpeg libavcodec-extra
	sudo apt install --no-install-recommends -y ubuntu-restricted-extras ubuntu-restricted-addons

	echo -e "#{color_red}\n============= Installing media apps =============#{color_off}\n"
	sudo apt install --no-install-recommends -y vlc mplayer youtube-dl
	
	echo -e "#{color_red}\n============= Installing password manager =============#{color_off}\n"
	sudo apt install --no-install-recommends -y keepassx

	echo -e "#{color_red}\n============= Installing Spotify =============#{color_off}\n"
    sudo snap install spotify
	
	echo -e "#{color_red}\n============= Installing Atom and Sublime-Text =============#{color_off}\n"
	sudo snap install atom
	sudo snap install sublime-text

	echo -e "#{color_red}\n============= Installing Dropbox =============#{color_off}\n"
	sudo apt install --no-install-recommends -y caja-dropbox

	echo -e "#{color_red}\n============= Installing graphical apps =============#{color_off}\n"
	sudo apt --no-install-recommends -y pinta gimp inkscape mypaint imagemagick shutter trimage

	echo -e "#{color_red}\n============= Installing filezilla =============#{color_off}\n"
	sudo apt --no-install-recommends -y filezilla
	
	echo -e "#{color_red}\n============= Installing command-line snippet manager =============#{color_off}\n"
	wget https://github.com/knqyf263/pet/releases/download/v0.3.1/pet_0.3.1_linux_amd64.tar.gz
    tar zxvf pet*.tar.gz
    sudo cp pet /usr/local/bin/
    sudo chmod +x /usr/local/bin/pet

	echo -e "#{color_red}\n============= Configuring language pt-br =============#{color_off}\n"
	sudo locale-gen pt_BR.UTF-8
    sudo sed -i -e 's/# pt_BR.UTF-8 UTF-8/pt_BR.UTF-8 UTF-8/' /etc/locale.gen
    sudo sed -i -e 's/# pt_BR ISO-8859-1/pt_BR ISO-8859-1/' /etc/locale.gen
	sudo sh -c "echo 'LANG=pt_BR.UTF-8'>/etc/default/locale"
    sudo dpkg-reconfigure --frontend=noninteractive locales
    sudo update-locale LANG=pt_BR.UTF-8
	sudo apt install --no-install-recommends -y language-pack-pt language-pack-pt-base

	echo -e "#{color_red}\n============= Configuring Time Zone America/Fortaleza =============#{color_off}\n"	
	sudo ln -snf /usr/share/zoneinfo/America/Fortaleza /etc/localtime
	sudo sh -c "echo 'America/Fortaleza'>/etc/timezone"
    sudo dpkg-reconfigure -f noninteractive tzdata
	
	echo -e "#{color_red}\n============= Configuring Abnt2 Keyboard =============#{color_off}\n"	
	sudo setxkbmap -model abnt2 -layout br -variant abnt2
	sudo dpkg-reconfigure -f noninteractive keyboard-configuration

	echo -e "#{color_red}\n============= Installing gscan2pdf =============#{color_off}\n"
	sudo apt-get install gscan2pdf
	
	echo -e "#{color_red}\n============= Installing notepadqq =============#{color_off}\n"
	sudo apt install --no-install-recommends -y  notepadqq notepadqq-gtk

	echo -e "#{color_red}\n============= Installing Visual Studio Code =============#{color_off}\n"
    sudo apt-get install code # or code-insiders
	
	echo -e "#{color_red}\n============= Configuring VirtualBox Guest Additions =============#{color_off}\n"
	sudo wget -c http://download.virtualbox.org/virtualbox/5.2.12/VBoxGuestAdditions_5.2.12.iso
	sudo mount VBoxGuestAdditions_5.2.12.iso -o loop /mnt
	cd /mnt
	sudo sh VBoxLinuxAdditions.run --nox11
	cd /tmp
	sudo rm *.iso
	sudo /opt/VBoxGuestAdditions-5.2.12/init/vboxadd setup

	git config --global color.ui true
	sudo chsh -s $(which zsh) $(whoami)
	
	echo -e "#{color_yellow}\n============= Finishing. Please Restart your Machine =============#{color_off}\n"
	
  SHELL

  config.vm.provision "ansible_local" do |a|
     a.verbose = true
     a.install = true
     a.install_mode = "pip"
     a.playbook = "setup.yml"
     a.verbose = "v"
	 a.provisioning_path = 'C:\vagrant'
  end
end
