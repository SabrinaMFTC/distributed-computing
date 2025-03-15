# Configuring AWS's Cluster

## Creating the instances (one manager, two workers)

1. Instances > Launch an instance
2. Name it - e.g. `manager`
3. Choose Ubuntu as OS
4. Create key pair and choose RSA (the private key will be downloaded)
5. Create security group > Allow SSH traffic from > Anywhere
6. Network settings > Add security group rule > Select `Type > All TCP` and `Source type > Anywhere`
7. Click on `Launch instance`
8. Repeat the process for the two workers (`worker1` and `worker2`), selecting the created security group

## Configuring the instances

`localHost`

```bash
cd /Downloads
chmod 400 <keyName>
eval `ssh-agent`
ssh-add <keyName>
ssh ubuntu@<instancePublicIP> -A
```

Add the instances IPs to the hosts file (repeat this process for all instances/machines)

`ubuntu@ip-<hostnamePrivateIP>`

```bash
sudo vim /etc/hosts

<managerPrivateIP> <hostName1> # e.g. manager
<worker1PrivateIP> <hostName2> # e.g. worker1
<worker2PrivateIP> <hostname3> # e.g. worker2
```

To change to another instance, you can use

```bash
ssh ubuntu@<instanceHostname> -A
```

## Install MPI on all instances

```bash
sudo apt update && sudo apt upgrade
sudo apt install openmpi-bin
sudo chmod +x $(which mpirun)
sudo chmod +x $(which orted)
mpirun --host worker1,worker2 hostname
```



