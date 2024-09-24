Primero ponemos "vagrant box add gusztavvargadr/ubuntu-server-2004-lts" para añadir la maquina virtual
Iniciamos el archivo de vagrant con "   vagrant init gusztavvargadr/ubuntu-server-2004-lts"
Ahora dentro del visual studio cambiamos la configuracion dentro del vagrant file
Pondriamos esto:
``` ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  config.vm.box = "gusztavvargadr/ubuntu-server-2004-lts"
  config.vm.hostname = "server-emm"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
  config.vm.synced_folder "./facto", "/data"
end
```