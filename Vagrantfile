
$script=<<-BUILD
clear
echo starting provision
sleep 2
clear
echo [+] adding repo
echo "deb https://packages.grafana.com/enterprise/deb stable main" >> /etc/apt/sources.list
sleep 2
clear
if [ $? -eq 0 ];then true; else echo "[!] something went wrong"; exit 1;fi
echo [+] updating repo cache
sleep 2
apt-get update &> /dev/null
clear
echo [+] repo updated
sleep 2 
echo [+] installing grafana 
apt-get install grafana -y  &> /dev/null
if [ $? -eq 0 ];then true; else echo "[!] something went wrong"; exit 1;fi
echo [+] starting the service
sudo systemctl start --now grafana
if [ $? -eq 0 ];then true; else echo "[!] something went wrong"; exit 1;fi
sleep 2
clear

BUILD



Vagrant.configure("2") do |config|
  config.vm.define "vb" do |vb|
    vb.vm.box = "generic/ubuntu1604"
    vb.vm.hostname = "pgg-stack"
    vb.vm.network "private_network", ip: "10.10.100.10"
    vb.vm.network "forwarded_port",  guest:80 , host:8080
    vb.vm.network "forwarded_port",  guest:9090 , host:9090
    vb.vm.network "forwarded_port",  guest:3000 , host:3000
    vb.vm.provision "shell", inline: $script
  end

end
