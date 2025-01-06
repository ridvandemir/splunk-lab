# splunk-lab

Objective

The Splunk Lab project aimed to establish a comprehensive environment for exploring Splunk’s data ingestion and analysis capabilities. The primary focus was on configuring Splunk components (Search Head, Indexer, and Universal Forwarders) to forward logs from Linux and Windows machines. The lab was designed to explore Splunk's diverse data input mechanisms, including monitoring files, running scripts to collect data and configuring network inputs like Firewall for real-time log ingestion.

Skills Learned

- Proficiency in configuring and managing Splunk components (Search Head, Indexer, and Universal Forwarder).
- Practical experience in forwarding logs from Linux and Windows systems.
- Understanding of various Splunk input possibilities, such as monitoring files and scripted inputs.
- Hands-on knowledge of setting up and configuring Splunk infrastructure for effective data collection and analysis.
- Enhanced ability to troubleshoot and optimize log ingestion workflows in Splunk.

Tools Used

- Virtual Machine to deploy lab environment
- Splunk Enterprise for log management, analysis, and querying.
- Splunk Universal Forwarder for log forwarding from Windows and Linux machines.
- PuTTY terminal emulator to connect to servers.

Network Diagram

![splunk_lab drawio](https://github.com/user-attachments/assets/259e2b7d-bd00-42ea-a4e0-bcb41c588ea6)

Steps

1-Installation and Setup of Virtual Machines
- For the Linux machines, Ubuntu 22.04 and for the Windows machine Windows 10 were installed on VirtuelBox as virtual machines.
- As a network ‘NAT Network' was selected and static IP address was determined on each virtual machine.
- After installing virtual machines, we saw that they were reachable with the ‘ping’ command.
- Splunk Enterprise was installed as Seach Head and Indexer on the Linux machine to monitor the logs.
- Universal Forwarder was installed on Linux and Windows 10 machine to send logs to Splunk.

2-Splunk Forwarder on Linux machines
- First, I changed the IP address to static IP.
  - sudo nano /etc/netplan/00-installer-config.yaml
  - IP:192.168.2.10, IP:192.168.2.11
- I installed Splunk Forwarder and ran it.
  - sudo dpkg -i <splunkforwarder .deb folder>
  - sudo –u splunkfwd bash #I switched to splunkfwd user
  - $SPLUNK_HOME/bin/splunk start –accept-license
- I activated 'boot start'.
  - sudo $SPLUNK_HOME/bin/splunk enable boot-start -user splunkfwd
- I planed to monitor scripted inputs from first Linux machine and security logs from second Linux machine. For this purpose, I created inputs.conf and outputs.conf under $SPLUNK_HOME/etc/apps/lab/local directory for each of them. I created this 'lab' under apps to manage forwarding effectively. Remember to place scripts under $SPLUNK_HOME/etc/apps/lab/bin directory. I used a log generating script for this lab.

3-Splunk Forwarder on Windows machine
- First, I changed the IP address to static IP.
  - Internet Settings>Change Adapter Options>Ethernet>Properties>IPv4 Properties>Manual
  - IP:192.168.2.12
- I installed Splunk Forwarder
  - Receiving Server> Host/IP: 192.168.2.20, Port:9997
- I then configured inputs.conf under $SPLUNK_HOME/etc/apps/lab/local directory

4-Indexer
- First, I changed the IP address to static IP.
  - sudo nano /etc/netplan/00-installer-config.yaml
  - IP:192.168.2.20
- I installed Splunk and ran it.
  - sudo dpkg -i <splunk .deb folder>
  - sudo –u splunk bash #I switched to splunk user
  - $SPLUNK_HOME/bin/splunk start –accept-license
- I activated 'boot start'.
  - sudo $SPLUNK_HOME/bin/splunk enable boot-start -user splunk
- I configured Indexer to get the logs from universal forwarders. For this purpose, I created inputs.conf and indexes.conf under $SPLUNK_HOME/etc/apps/lab/local directory.

5-Search Head
- First, I changed the IP address to static IP.
  - sudo nano /etc/netplan/00-installer-config.yaml
  - IP:192.168.2.21
- I installed Splunk and ran it.
  - sudo dpkg -i <splunk .deb folder>
  - sudo –u splunk bash #I switched to splunk user
  - $SPLUNK_HOME/bin/splunk start –accept-license
- I activated 'boot start'.
  - sudo $SPLUNK_HOME/bin/splunk enable boot-start -user splunk
- Now that I installed Search Head on a single machine, I must connect Indexer to Searh Head. So that I can run the search query in my Indexer. For this purpose, I created distsearch.conf along with inputs.conf and web.conf under $SPLUNK_HOME/etc/apps/lab/local directory.
