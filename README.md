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

![splunk_lab](https://github.com/user-attachments/assets/3ad378b2-500b-481a-ba74-b2f689f43fa5)

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
- I planed to monitor scripted inputs from first Linux machine and security logs from second Linux machine. So, I created inputs.conf and outputs.conf for both of them. I also created 'lab' under apps to manage forwarding effectively. Scripts should be under $SPLUNK_HOME/etc/apps/lab/bin directory. I used a common script for this lab.
  - For the first Linux (IP:192.168.2.10) 
  - /opt/splunkforwarder/etc/apps/lab/inputs.conf
    - [script://./bin/log_generate.py]
    - interval=120
    - sourcetype=script
    - disabled=0
    - index=linux

3-Splunk Forwarder on Wondows machine
- First, I changed the IP address to static IP.
  - Internet Settings>Change Adapter Options>Ethernet>Properties>IPv4 Properties>Manual
  - IP:192.168.2.12
- I installed Splunk Forwarder
  - Receiving Server> Host/IP: 192.168.2.20, Port:9997

