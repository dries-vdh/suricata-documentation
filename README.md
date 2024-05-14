# Ontketen de Kracht van Intrusiedetectie

## Introductie
In dit artikel wordt er praktisch besproken hoe Suricata als IDS kan toegepast worden in een netwerkomgeving. Suricata is een open-source IDS die gebruik maakt van signature-based detection. Dit betekent dat Suricata aan de hand van vooraf gedefinieerde regels (signatures) kan detecteren of er zich verdachte activiteiten voordoen in het netwerk. Suricata is een van de meest gebruikte IDS'en en is zeer populair in zowel kleine als grote netwerkomgevingen.

## Installatie
Om Suricata te installeren op een Ubuntu 20.04 systeem, kan je de volgende stappen volgen.
    
```bash
sudo apt update && \
sudo apt upgrade -y

sudo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt update
sudo apt install suricata jq
```
