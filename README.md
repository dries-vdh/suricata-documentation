# Ontketen de Kracht van Intrusiedetectie

## Introductie
In dit artikel wordt er praktisch besproken hoe Suricata als IDS kan toegepast worden in een netwerkomgeving. Suricata is een open-source IDS die gebruik maakt van signature-based detection. Dit betekent dat Suricata aan de hand van vooraf gedefinieerde regels (signatures) kan detecteren of er zich verdachte activiteiten voordoen in het netwerk. Suricata is een van de meest gebruikte IDS'en en is zeer populair in zowel kleine als grote netwerkomgevingen.

## Installatie
Om Suricata te installeren op een Debian 12 systeem, kan je de volgende stappen volgen.
    
```bash
sudo apt update && \
sudo apt upgrade -y

sudo apt-get install software-properties-common -y
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt update
sudo apt install suricata jq -y
```

De installatie van Suricata is nu voltooid. Om te controleren of Suricata correct geïnstalleerd is, kan je de volgende commando's uitvoeren.

```bash
sudo suricata --build-info
sudo systemctl status suricata
```

## Basic Suricata configuration

Kijk welke interface je wil monitoren met het *ip a* commando. Gebruik deze informatie om Suricata te configureren.

```bash
sudo vim /etc/suricata/suricata.yaml
```

Om de interface te configureren, zoek naar de *af-packet* sectie en pas de *interface* waarde aan.

```yaml
af-packet:
  - interface: ens33
... 
```

De default configuratie volstaat om een intern netwerk te monitoren. Daarom zal er niet verder worden ingegaan op de configuratie van Suricata.

## Suricata signatures activeren

Om Suricata signatures te activeren, kan je volgend commando uitvoeren.

```bash
sudo suricata-update
```
