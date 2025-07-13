# Introduction

 This project presents an extensive cybersecurity monitoring and
 incident response framework developed with open-source tools. The
 setup includes several virtual machines designed to identify, analyze,
 and react to security threats in real-time. Key elements of the system
 include Wazuh for log monitoring and analysis, TheHive for managing
 incidents, and Sysmon for in-depth system event tracking. To
 streamline automated actions like blocking harmful IP addresses and
 issuing alerts, I incorporated Shuffle. The project highlights an
 advanced, automated method for detecting and responding to threats
 within a secure, virtualized lab environment.

# Tools used

- **Wazuh**: A security monitoring platform that provides log analysis,
intrusion detection, and compliance monitoring.

- **Sysmon**: A Windows system monitoring tool that logs detailed event
data for security and forensic analysis.

- **TheHive**: An open-source incident response platform designed to
manage and analyze security incidents.

- **Shuffle**: An automation tool for orchestrating security workflows
and triggering automated responses to incidents.

- **Mimikatz**: A tool used for extracting passwords and other
credentials from Windows systems, often for penetration testing.

- **DigitalOcean**: A cloud service provider offering scalable virtual
machines and other cloud-based infrastructure services.

# Skills aquired

- Virtual Machine Deployment and Management (DigitalOcean)

- Configuring Firewalls (Implementing custom rules to secure VMs)

- Sysmon Setup (Logging Windows system events)

- Wazuh Setup and Configuration (Custom rule creation and log
  monitoring)

- Incident Management Configuration (Utilizing TheHive to handle
  security alerts)

- Automating with Shuffle (Enabling automatic actions like IP blocking)

- Integrating Security Tools (Connecting Wazuh, TheHive, Sysmon, Shuffle
  into a cohesive system)

- Developing Custom Rules (Creating specific rules in Wazuh for tailored
  alerts)

- Automated Email Notifications (Setting up automated alerts for
  security incidents)

# Table of contents

[I. Introduction [1](#introduction)]

[II. Tools used [1](#tools-used)]

[III. Skills aquired [1](#skills-aquired)]

[IV. Table of contents [2](#table-of-contents)]

[V. Project [2](#project)]

[1. Infrastructure Design
[2](#infrastructure-design)]

[2. Installation of Sysmon on Windows 10 virtual machine
[3](#_Toc198983931)]

[3. Wazuh virtual machine and firewall configuration
[4](#wazuh-virtual-machine-and-firewall-configuration)]

[4. Wazuh Setup [5](#wazuh-setup)]

[5. TheHive Setup [5](#thehive-setup)]

[6. Generating alerts [9](#generating-alerts)]

[7. Integrating Shuffle
[13](#integrating-shuffle)]

[8. Incident Response [19](#incident-response)]

[9. Summary [21](#summary)]

[10. Sources [21](#sources)]

# Project

## Infrastructure Design  

The initial phase of the project focused on designing a visual
representation of the lab\'s architecture. This diagram acts as a
structural guide, illustrating how tools such as Wazuh, TheHive, and
Sysmon are connected and interact within the network environment.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_1.png](images/Screenshot_1.png)

## Installation of Sysmon on Windows 10

Once I had a clear view on what the architecture will look like, a
proceeded to install Sysmon on Windows 10 virtual machine. Sysmon
enhances the level of detail in the Windows logs. The installed and
active Sysmon service can be seen in „Services" window.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_2.png](images/Screenshot_2.png)

The enhanced logging will be very useful when we will integrate wazuh
into our infrastructure for real-time analysis.

## Wazuh virtual machine and firewall configuration

Few of the most crutial conponents of this lab were housed in Virtual
Machines. I decided to use DigitalOcean for hosting. First I created a
Wazuh virtual machine that will serve as a SIEM (Security Information
and Event Management) platform. Wazuh is a powerful open-source security
monitoring tool that provides comprehensive threat detection and
response capabilities.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_3.png](images/Screenshot_3.png)

For security purposes I configured a firewall so that only I can connect
to the virtual machines and services hosted on them. This prevents
unauthorized access and adds an essential layer of security to the
environment.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_4.png](images/Screenshot_4.png)

## Wazuh Setup

I started by connecting to my newly created VM in powershell, via SSH.
Then I proceeded with Wazuh Installation.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_5.png](images/Screenshot_5.png)

After successfuly finishing the installation, I was able to connect and
log into Wazuh Dashboard through the web browser.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_6.png](images/Screenshot_6.png)

## TheHive Setup

Now I proceeded to create another Virtual Machine in Digital Ocean. This
one with the purpose of housing TheHive, an incident response platform.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_7.png](images/Screenshot_7.png)

And the first thing I did after creating this VM, was adding It to my
firewall for protection.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_8.png](images/Screenshot_8.png)

As with the Wazuh machine, I started with connecting to this one through
SSH.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_9.png](images/Screenshot_9.png)

Then I installed the core components that TheHive relies on to manage
data and proces alerts. The components are: Cassandra, Java,
EasticSearch and TheHive.

I started with Cassandra configuration. I did it by modifying
/etc/cassandra/cassandra.yaml file. Cassandra is a widely adopted
open-source NoSQL distributed database known for delivering high
scalability and availability while maintaining strong performance,
making it a reliable choice for many organizations.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_10.png](images/Screenshot_10.png)

Now that Cassandra is working, I proceeded with ElasticSearch
configuration. I did it by setting up things like cluster name, node
name and networkhost in /etc/elasticsearch/elasticsearch.yaml file.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_11.png](images/Screenshot_11.png)

With the core components up and running I could start TheHive
configuration. I started with changing ownership over thehive lib. It
wouldn't work without it.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_13.png](images/Screenshot_13.png)

Then I started configurating TheHive, by modifying
/etc/thehive/application.conf file. I setup the hostname field with my
TheHive VM's IPv4 address, and cluster-name field with my cluster name
that I defined previously.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_14.png](images/Screenshot_14.png)

I also changed the application.baseUrl field for TheHive url.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_15.png](images/Screenshot_15.png)

TheHive was running and I was able to connect to TheHive Dashboard.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_16.png](images/Screenshot_16.png)

## Generating alerts

First I made my Windows 10 VM a Wazuh agent, so that it would send logs
to my Wazuh machine for analysis.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_17.png](images/Screenshot_17.png)

Next I needed to begin generating telemetry data from the Windows 10
Virtual Machine. I focused on capturing logs related to process
creation, network events and security anomalies. This was achieved by
modify the ossec.conf file to exclude unnecessary logs and Focus on
Sysmon logs.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_18.png](images/Screenshot_18.png)

Then I downloaded Mimikatz, a popular post-exploitation tool, to
simulate an attack by extracting credentials from the machine. To do
that, I needed to exclude Downloads folder from my Windows 10 firewall.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_19.png](images/Screenshot_19.png)

I used Mimikatz to test the threat detection capabilities of my setup,
ensuring Wazuh would raise alerts when malicious activity occurred.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_20.png](images/Screenshot_20.png)

To be able to recieve alerts, I modified the /etc/filebeat/filebeat.yaml
file, changing the enabled field to true.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_21.png](images/Screenshot_21.png)

At this point I entered the Wazuh Dashboard and created an index pattern
called „wazuh-archives" to collect archived logs.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_22.png](images/Screenshot_22.png)

Everything was working and the logs were properly collected in the
archives folder. I could search through them and look for Mimikatz. As
seen bellow, we have some alerts.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_23.png](images/Screenshot_23.png)

Next I proceeded to create a new local rule for mimikatz. I achived it
by modifyig local_rules file as shown bellow.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_24.png](images/Screenshot_24.png)

I gave it rule_id 100002 and threat level 15 witch is the highest.

As we can see in Wazuh Dashboard, the rule works.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_25.png](images/Screenshot_25.png)

## Integrating Shuffle

At this stage I began to integrate Shuffle into my infrastructure.
Shuffle is an automation platform that will connect Wazuh with other
tools, enabling automated responses to security events.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_26.png](images/Screenshot_26.png)

After creating an empty Shuffle environment and adding a webhook trigger
mechanism, I began to connect Shuffle with my Wazuh Manager. This was
made by adding \<Integration\> section with my shuffle webhook to the
/var/ossec/etc/ossec.conf file on my Wazuh Manager.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_27.png](images/Screenshot_27.png)

Now we can see events from Wazuh.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_28.png](images/Screenshot_28.png)

I wanted to verify Wazuh events with Virustotal before sending them
further. To to that I created a custom Regex function that would extract
a hash and this hash would be send to Virustotal.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_29.png](images/Screenshot_29.png)

Then I added Virustotal to my workflow.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_30.png](images/Screenshot_30.png)

Next step was adding TheHive to my workflow.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_34.png](images/Screenshot_34.png)

Before I could configure TheHive in Shuffle, I needed to configure
TheHive itself. I did it by adding a new user that would be responsible
for recieving alerts.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_31.png](images/Screenshot_31.png)

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_32.png](images/Screenshot_32.png)

I alse had to allow TheHive in my firewall.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_33.png](images/Screenshot_33.png)

After that I configured the Shuffle TheHive addon and everything was
working. I was Able to recieve alerts in TheHive.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_35.png](images/Screenshot_35.png)

Next was adding email notifications about potential threats. I did it by
adding email app to my Shuffle workflow.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_38.png](images/Screenshot_38.png)

The email was send succesfuly.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_37.png](images/Screenshot_37.png)

## Incident Response

The final stage of my project was to create response action. The
objective was to receive email and being able to block ip address that
was not friendly for me.

First thing I did was creating an active response in
/var/ossec/etc/ossec.conf file on my Wazuh Manager.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_39.png](images/Screenshot_39.png)

I also modified my Shuffle workflow.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_41.png](images/Screenshot_41.png)

I added firewall-drop0 command to my Wazuh app on Shuffle and for
testing purposes I used google DNS to block.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_40.png](images/Screenshot_40.png)

Everything was working to I changed the 8.8.8.8 to attackers IP address.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_42.png](images/Screenshot_42.png)

Next step was adding the feature of blocking unwanted IPs. I achieved it
by utilizind User Input app.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_44.png](images/Screenshot_44.png)

As shown below, everything was working.

![C:\Users\Admin\Desktop\Cybersecurity SOC Automation
Project\Screenshot_43.png](images/Screenshot_43.png)

## Summary

 The SOC Automation Lab project showcases the deployment of a
 comprehensive, automated cybersecurity monitoring and incident
 response system built with open-source tools. Core components include
 Wazuh for log monitoring and analysis, Sysmon for detailed system
 event tracking, and TheHive for managing incidents. Shuffle was
 integrated to automate response actions such as blocking malicious IP
 addresses and sending alerts.

 The project included setting up virtual machines, configuring
 firewalls, and optimizing both Wazuh and TheHive for real-time
 telemetry and alerting. Custom detection rules were created in Wazuh
 to identify simulated attacks, and Shuffle workflows were developed to
 automate the response process. Additionally, Cortex and MISP were
 incorporated to provide threat intelligence and enrich event data,
 forming a cohesive framework for real-time threat detection and
 response.

## Sources

- [Wazuh
  Documentation](https://documentation.wazuh.com/current/index.html)

- [TheHive Documentation](https://docs.strangebee.com/)

- [Shuffle Documentation](https://shuffler.io/docs/about)

- [Mydfir Channel](https://www.youtube.com/@MyDFIR)
