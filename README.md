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

The default location to the rules files should also be changed.

The default configuration is sufficient to monitor an internal network. Therefore, we will not go into further detail on the configuration of Suricata.

## Activating Suricata Signatures

To activate Suricata signatures, you can run the following command:

```bash
sudo suricata-update
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
