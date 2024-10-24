# PR0102: Entornos multimáquina
## Instalar maquinas virtuales
```powershell
vagrant box add StefanScherer/windows_2019
vagrant box add StefanScherer/windows_10
```
## Vagrant file windows server
```ruby
Vagrant.configure("2") do |config|

  config.vm.box = "StefanScherer/windows_2019"
  config.vm.hostname = "server-emm"
  config.vm.network "private_network" , ip: "172.16.0.1"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 4
  end
end
```
## Vagrant file windows 10
```ruby
Vagrant.configure("2") do |config|

  config.vm.box = "StefanScherer/windows_10"
  config.vm.hostname = "w10-emm"
  config.vm.network "private_network" , ip: "172.16.0.10"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2 
  end
end
```
## Conectar las maquinas
```powershell
vagrant rdp server-emm
vagrant rdp w10-emm
```