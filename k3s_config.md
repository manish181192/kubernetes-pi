Follow the guide here for more details: https://rpi4cluster.com/k3s/k3s-kube-setting/


# Pre-Installation on all Devices

1. Turn off Swap memory on each device
* ```$ sudo swapoff -a ``` (undo: sudo swapon -a)
* ```$ sudo nano /etc/dphys-swapfile``` Edit following
```
CONF_SWAPSIZE=0
```

2. Add CGroup for your raspberry pi
* ```$ sudo nano /boot/firmware/cmdline.txt```
2. Append below into THE END of the line
```
cgroup_enable=cpuset cgroup_memory=1 cgroup_enable=memory
```

3. Set root password on your device
* use device as root ``` $ sudo su -```
* ```passwd```

4. REBOOT the devices
```$ sudo reboot```

# Pre-Installation on MASTER
1. Login as root
2. Install ansible ```$ apt install ansible```. 
* Setup /etc/ansible/hosts using this guide - https://rpi4cluster.com/k3s/k3s-nodes-setting/

# K3s Installation
1. Install Master node
* Select a master node, login as root
* Install K3s - ```$ curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE="644" sh -s - --disable local-storage --node-taint```
* Find your K3s Token here ``` $ cat /var/lib/rancher/k3s/server/node-token```

2. Install on worker nodes
* Install worker K3s -- ```$ curl -sfL https://get.k3s.io | K3S_TOKEN="<<MY_TOKEN_FROM_PREVIOUS_STEP>>" K3S_URL="https://<MASTER_IP_ADDRESS>:6443" K3S_NODE_NAME="pi2" sh -```
