bootstrap = <<SCRIPT
  useradd -m hackrf --groups sudo && echo "hackrf:hackrf" | chpasswd && su -c "printf 'cd /home/hackrf\nsudo su hackrf' >> .bash_profile" -s /bin/sh ubuntu
  usermod -a -G plugdev hackrf
  useradd -m codergv --groups sudo && echo "codergv:codergv" | chpasswd && su -c "printf 'cd /home/codergv\nsudo su codergv' >> .bash_profile" -s /bin/sh ubuntu
  usermod -a -G plugdev codergv
  apt-get install git
  git clone https://github.com/Drew-CodeRGV/CodeRGV_spectrum_painter.git
  mv /home/vagrant/CodeRGV_spectrum_painter /home/hackrf/CodeRGV_spectrum_painter
  cd /home/hackrf/CodeRGV_spectrum_painter
  chmod 777 codergv.sh
  chmod 777 codergv-lp.sh
  chmod 777 jam.sh
  wget https://www.dropbox.com/s/603500m4xiozw8u/codergv.iqhackrf?dl=0
  mv codergv.iqhackrf?dl=0 CodeRGV_spectrum_painter/codergv.iqhackrf
  wget https://www.dropbox.com/s/vtmjrzzgvhgor01/jam.iqhackrf?dl=0
  mv jam.iqhackrf?dl=0 CodeRGV_spectrum_painter/jam.iqhackrf
  sudo ap-get install python-pip
  pip install click
  python pip install numpy scipy matplotlib ipython jupyter pandas sympy nose
  apt-get update
  apt-get install -y ubuntu-desktop gnuradio hackrf gr-osmosdr
  echo ATTR{idVendor}=="1d50", ATTR{idProduct}=="6089", SYMLINK+="hackrf-one-%k", MODE="660", GROUP="plugdev" > /etc/udev/rules.d/52-hackrf.rules
  udevadm control --reload-rules
  mv /home/vagrant/dfs_pulse_tester.grc /home/hackrf/dfs_pulse_tester.grc && chown hackrf.hackrf /home/hackrf/*.grc


SCRIPT

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/xenial64"

  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = true
  end

  config.vm.provider "virtualbox" do |vb|
    vb.name = "CodeRGV HackRF"
    vb.memory = "4096"
    vb.customize ["modifyvm", :id, "--vram", "32"]
    vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
    vb.customize ["modifyvm", :id, "--usb", "on"]
    vb.customize ["modifyvm", :id, "--usbehci", "on"]
    vb.gui = true
  end

  config.vm.provision :file, source: "dfs_pulse_tester.grc", destination: "dfs_pulse_tester.grc"
  config.vm.provision "shell", inline: "#{bootstrap}", privileged: true

end
