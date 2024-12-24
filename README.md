# OpenFlow SDN with Ryu Controller Integrated with Snort (Using GNS3 Environment)

## Objective

This project demonstrates the integration of **OpenFlow SDN** using the **Ryu controller** with **Snort**, a network intrusion detection system (NIDS), for mitigating Distributed Denial of Service (DDoS) attacks in a network environment. The project uses **GNS3** to simulate the network topology and **Ubuntu Server** to host the Ryu controller and Snort NIDS. The goal is to showcase how SDN can dynamically detect and mitigate various types of network attacks, including ICMP, TCP, and UDP floods, using Ryu’s flow management and Snort’s attack detection capabilities.

![image](https://github.com/user-attachments/assets/94413e5f-567d-4225-86aa-38fd95b46d8e)

---

## Skills Learned

- Understanding of **OpenFlow** and **SDN** architecture.
- Configuring **Ryu controller** and integrating it with OpenFlow-enabled devices.
- Installing and configuring **Snort** as an **Intrusion Detection System**.
- Using **GNS3** for creating and managing network simulations.
- Implementing dynamic network traffic control for **DDoS mitigation**.
- Working with network mirroring and monitoring traffic for attack detection.

![image](https://github.com/user-attachments/assets/b8a8e450-cd9f-4802-a9f0-ab6510ea26da)

---

## Tools Used

- **GNS3**: For simulating the network topology with Cisco switches, routers, and virtual machines.
- **Ryu Controller**: For managing SDN flow tables and traffic routing.
- **OpenvSwitch (OVS)**: For creating virtual switches and mirroring traffic to Snort.
- **Snort**: Network Intrusion Detection System (NIDS) used for detecting and alerting on DDoS attacks.
- **Ubuntu Server**: Hosting the Ryu controller, Snort, and other necessary components.
- **Hping3**: For simulating DDoS attacks in the network.

![image](https://github.com/user-attachments/assets/4fac569d-212f-4180-ba64-fd94bafb8cc3)

---

## Steps

### 1. GNS3 Topology Setup

The GNS3 workspace was populated with the following devices:

- Four **Cisco switches** for the core network switches.
- One **Cisco router** for routing between networks.
- Ten **host PCs** connected to the switches for testing traffic generation.
- One **OpenvSwitch (OVS)** integrated into the topology to work with the Ryu controller.
- **Ryu Controller** running on an Ubuntu server, responsible for managing traffic and flows.
- **Snort IDS** installed on the Ubuntu server to monitor and analyze network traffic.


---

### 2. Configuring OpenvSwitch (OVS)

OpenvSwitch (OVS) was installed on the Ubuntu server and configured to act as the central switch in the SDN network. To enable OVS and establish communication with the Ryu controller, the following commands were used:

```bash
# Install OVS
sudo apt install openvswitch-switch

# Start OVS service
sudo service openvswitch-switch start

# Create a bridge and add the interface
sudo ovs-vsctl add-br br0
sudo ovs-vsctl add-port br0 eth0

# Set up traffic mirroring for Snort
ovs-vsctl -- --id=@p get port br0 -- --id=@mirror create mirror name=snort-mirror select-all=true output-port=@p -- -- set Bridge br0 mirrors=@mirror

# Link OVS to the Ryu controller
sudo ovs-vsctl set-controller br0 tcp:192.168.234.236:6633
```

![image](https://github.com/user-attachments/assets/bb4cce95-15aa-4dc3-9d7b-49d876c304e3)


---

### 3. Ryu Controller Setup

Ryu was installed on the Ubuntu server using the following commands:

```bash
# Install necessary packages
sudo apt update
sudo apt install python3 python3-pip xterm iperf hping3 net-tools wireshark apache2-utils curl

# Install Ryu
sudo pip3 install ryu

# Verify installation
ryu-manager --version

# Run Ryu application
ryu-manager ryu-app.py
```

![image](https://github.com/user-attachments/assets/427d09a0-4afb-4cd8-a7e3-f2e86eaae48e)



---

### 4. Snort Installation and Configuration

Snort was installed and configured as follows:

```bash
# Install Snort
sudo apt-get update
sudo apt-get install snort

# Configure Snort to monitor traffic
sudo snort -A console -c /etc/snort/snort.conf -i eth0
```
![image](https://github.com/user-attachments/assets/84a1ab73-8eb5-49fa-bf55-6182185db176)

---

### 5. DDoS Attack Simulation with Hping3

To simulate DDoS attacks, the following commands were used:

#### ICMP Flood
```bash
hping3 --icmp --flood --rand-source <destination_ip>
```

#### TCP Flood
```bash
hping3 --flood -S --rand-source -p 80 <destination_ip>
```

#### UDP Flood
```bash
hping3 --udp --flood --rand-source -p 161 <destination_ip>
```

![image](https://github.com/user-attachments/assets/e884aa66-0081-4c5b-a121-e8b2328c254a)


---

### 6. Integration of Ryu and Snort for DDoS Mitigation

1. **Network Setup**: Ryu controller and Snort are configured to work together on the same server.
2. **Traffic Mirroring**: OVS mirrors all network traffic to Snort for inspection.
3. **Attack Detection**: Snort analyzes the incoming traffic for known attack patterns (ICMP, TCP, UDP floods).
4. **Alert Generation**: Upon detection, Snort generates an alert and sends it to the Ryu controller.
5. **Dynamic Flow Modification**: Ryu receives the alert and modifies the network flow table to mitigate the attack by blocking or rerouting malicious packets.


---

### 7. Experiment Results

Various types of DDoS attacks were simulated. The integration of Snort and Ryu provided an effective mitigation strategy. Dynamic flow control enabled the SDN network to detect and block malicious traffic in real-time, ensuring the availability and security of the network.

![image](https://github.com/user-attachments/assets/8c013047-c19e-4a89-982c-2a398a30325d)

---

### Conclusion

This project showcases how OpenFlow SDN and Ryu controller, when integrated with Snort IDS, can effectively mitigate DDoS attacks. By dynamically adjusting traffic flows based on attack detection, the system can prevent network disruptions and maintain service continuity.

For any questions or further assistance for the python code, please contact me - 📧 **Email**: [johnusiabulu@cipherknights.com](mailto:johnusiabulu@cipherknights.com) .

### Wireshark analysis in the network during DDOS attacks.
![image](https://github.com/user-attachments/assets/75e98f86-7758-4b5a-aa83-4334aa033502)

### Wireshark analysis in the network after DDOS attacks mitigated.
![image](https://github.com/user-attachments/assets/ac59e549-d986-46b8-8f75-16b7cb5908de)

### OpenVswitch Flow table showing packet dropped.
![image](https://github.com/user-attachments/assets/40ceb622-5a2b-4dd8-a0f0-6592b33cae4b)




