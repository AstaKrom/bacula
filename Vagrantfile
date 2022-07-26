Vagrant.configure("2") do |config|
 config.vm.box = "debian/buster64" #используемый образ
 config.vm.box_check_update = false #выключение обновления образа

 # Create first VM
 config.vm.define "bacula" do |bacula| # Имя первого уровня. Как понимает vagrant
   bacula.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 1
    v.name = "bacula" # Имя второго уровня. Как отображается в VirtualBox
   end
  bacula.vm.hostname = "bacula" #Имя третьего уровня. Отображение при подключении по ssh
  bacula.vm.network "private_network", ip: "192.168.56.13" # NAT + сеть VirtualBox
 end

 # Create second VM
 config.vm.define "rsync" do |rsync| # Имя первого уровня. Как понимает vagrant
   rsync.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 1
    v.name = "rsync" # Имя второго уровня. Как отображается в VirtualBox
   end
  rsync.vm.hostname = "rsync" #Имя третьего уровня. Отображение при подключении по ssh
  rsync.vm.network "private_network", ip: "192.168.56.14" # NAT + сеть VirtualBox
 end

end