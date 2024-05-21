# Unleash the Power of Intrusion Detection

## Introduction
This article provides a practical discussion on how to apply Suricata as an IDS in a network environment. Suricata is an open-source IDS that uses signature-based detection. This means that Suricata can detect suspicious activity in the network based on predefined rules (signatures). Suricata is one of the most widely used IDSs and is very popular in both small and large network environments.


## Installation
To install Suricata on a Debian 12 system, you can follow these steps:
    
```bash
sudo apt update && \
sudo apt upgrade -y

sudo apt-get install software-properties-common -y
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt update
sudo apt install suricata jq -y
```

The installation of Suricata is now complete. To check if Suricata is installed correctly, you can run the following commands:

```bash
sudo suricata --build-info
sudo systemctl status suricata
```

The service should fail to start because the configuration is not yet complete.

## Basic Suricata configuration

Check which interface you want to monitor with the ip a command. Use this information to configure Suricata.

```bash
sudo vim /etc/suricata/suricata.yaml
```

To configure the interface, look for the af-packet section and change the interface value.

```yaml
af-packet:
  - interface: ens33
... 
```

Make sure to replace ens33 with the network interface you want to monitor.

The default configuration is sufficient to monitor an internal network. Therefore, we will not go into further detail on the configuration of Suricata.

## Activating Suricata Signatures

To activate Suricata signatures, you can run the following command:

```bash
sudo suricata-update
```

Make sure the default location to the rules files is */var/lib/suricata/rules/suricata.rules*. You can change this in the configuration file.

```yaml
default-rule-path: /var/lib/suricata/rules
rule-files:
  - suricata.rules
... 
```

## Starting Suricata

To restart Suricata, you can use systemctl.

```bash
sudo systemctl restart suricata
```

To check if Suricata is running correctly, you can view the logs.

```bash
sudo tail -f /var/log/suricata/suricata.log
```

## Viewing Alerts

To view alerts, you can run the following command:

```bash
sudo tail -f /var/log/suricata/fast.log
```

## Implementing other rulesets

Suricata makes it very easy to implement rulesets. The different rulesets can be viewed with the following commands:
    
```bash
sudo suricata-update update-sources
sudo suricata-update list-sources
```

In this example, the Proofpoint open rules will be implemented.

```bash
sudo suricata-update enable-source et/open
sudo suricata-update
```

Then the Suricata service must be restarted.

After the service is reloaded, the new rules will be active. This can be checked by running the following commands:

```bash
sudo suricatasc

suricatasc> ruleset-stat
```

## Using Suricata with a mirror port

To use Suricata with a mirror port, you need to enable promiscuous mode on the network interface. This can be done with the following command:

```bash
sudo /usr/sbin/ip link set dev ens37 promisc on
```

Make sure to replace ens37 with the network interface you want to monitor.

## Promiscuous mode service

To enable promiscuous mode on boot, you can create a systemd service. Create a new file with the following command:

```bash
sudo vim /etc/systemd/system/promisc.service
```

Add the following content to the file:

```toml
[Unit]
Description=Enable promiscuous mode
Wants=suricata.service
After=suricata.service

[Service]
Type=oneshot
RemainAfterExit=no
User=root
ExecStart=/usr/sbin/ip link set dev ens37 promisc on 

[Install]
WantedBy=multi-user.target
```

Make sure to replace ens37 with the network interface you want to monitor.

