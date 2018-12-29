
# System

## Install python requirements
```
sudo pip install --upgrade -r requirements.txt
```

## Copy raspbian SO to SD
```
curl -sL https://downloads.raspberrypi.org/raspbian_lite_latest -o raspbian.zip
unzip raspbian.zip
dd if=2018-11-13-raspbian-stretch-lite.img | pv | dd of=/dev/mmcblk0 bs=4096
```

# Networking

## LAN dhcp static IP setup
* rpi-cl-01 B8:27:EB:BB:E0:95 192.168.0.101
* rpi-cl-02 B8:27:EB:32:EA:62 192.168.0.102

# Deployment

## Create swarm cluster
```
ansible-playbook playbooks/play.yml
```

## Deploy and test service
```
ansible-playbook playbooks/test.yml

### On manager node
docker service logs -f test

while true;do curl -s 192.168.0.101:8080 -o/dev/null -w "%{http_code} %{time_total}\n";done
```

## Deploy portainer server and agents
```
ansible-playbook playbooks/portainer.yml
```