# Setting Up SSH and Running ROS 2 on Multiple Machines (Using `ifconfig`)

This guide provides instructions for installing and configuring SSH, setting up network connectivity using `ifconfig`, and running ROS 2 on multiple machines.

## 1. Prerequisites
- Two or more machines running a Linux distribution (e.g., Ubuntu 20.04 or 22.04) with **ROS 2 installed**.
- Same **ROS 2 version** across all machines.
- **A shared network** (LAN/Wi-Fi) where all machines can communicate.
- **net-tools package** for `ifconfig` (install with: `sudo apt install net-tools`).

## 2. Installing and Configuring SSH
### 2.1 Install SSH
```bash
sudo apt update
sudo apt install openssh-client openssh-server
```
### 2.2 Enable and Start SSH Service
```bash
sudo systemctl enable ssh
sudo systemctl start ssh
systemctl status ssh
```
### 2.3 Test SSH Connection
From another machine:
```bash
ssh username@<SERVER_IP_ADDRESS>
```
Replace `<SERVER_IP_ADDRESS>` with the target machine’s IP.

## 3. Network Setup using `ifconfig`
### 3.1 Find Machine IP Address
```bash
ifconfig
```
Look for `inet` under the network interface (e.g., `eth0`, `wlan0`).
Example output:
```
eth0: inet 192.168.1.10  netmask 255.255.255.0
```

## 4. Running ROS 2 on Multiple Machines
### 4.1 Configure ROS 2 Environment
On each machine, add this to `~/.bashrc`:
```bash
source /opt/ros/<ROS2_DISTRO>/setup.bash
export ROS_DOMAIN_ID=<PREFER_NUMBER>
```
Replace `<ROS2_DISTRO>` (e.g., `humble`, `galactic`).

Replace `<PREFER_NUMBER>` with your prefer numbers.

## 5. Testing ROS 2 Communication
### On Machine A:
```bash
ros2 run demo_nodes_cpp talker
```
### On Machine B:
```bash
ros2 run demo_nodes_cpp listener
```
Machine B should receive messages:
```
[INFO] [listener]: I heard: [Hello World: 1]
```

## 6. Running ROS 2 Over SSH
### SSH into Machine B:
```bash
ssh userB@192.168.1.11
```
### Start ROS 2 Listener on Machine B:
```bash
source /opt/ros/<ROS2_DISTRO>/setup.bash
export ROS_DOMAIN_ID=0
ros2 run demo_nodes_cpp listener
```
### Start Talker on Machine A:
```bash
ros2 run demo_nodes_cpp talker
```

## 7. Troubleshooting
- **Firewall Issues**:  
  ```bash
  sudo ufw disable
  ```
- **Wrong Network Interface**: Configure DDS if needed.
- **Mismatched `ROS_DOMAIN_ID`**: Ensure all machines use the same value.

## 8. Summary
1. **Install SSH** and verify connectivity.
2. **Check IP addresses** using `ifconfig`.
3. **Verify network connectivity** with `ping`.
4. **Configure ROS 2** on all machines.
5. **Run talker/listener** to confirm multi-machine communication.


