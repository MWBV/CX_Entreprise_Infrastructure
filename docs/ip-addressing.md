---

# Plan d'Adressage IP & Segmentation VLANs

Ce document détaille la segmentation et la repartissions des blocs d'addresses IP pour l'ensemble des sites de l'entreprise CX. L'adressage repose sur le sous-réseau privé `10.0.0.0/8`, découpé par site.

---

## 1. Structuration Globale des Sites

* **QG (Côte d'Ivoire) :** `10.10.0.0/16`
* **Branch 1 (Côte d'Ivoire Annexe) :** `10.11.0.0/16`
* **Branch 2 (Afrique du Sud) :** `10.12.0.0/16`
* **Réseau de Transit Inter-Firewalls :** `192.168.100.0/29`

---

## 2. Table d'Adressage Détaillée du QG (`10.10.0.0/16`)

| VLAN ID | Nom du VLAN | Sous-Réseau | Passerelle (FW-Core) | Plage DHCP | Usage / Affectation |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **VLAN 10** | `DEV-CODE` | `10.10.10.0/24` | `10.10.10.1` | `10.10.10.100 - .200` | Postes Développeurs & Staging local |
| **VLAN 20** | `GRAPHIC-DESIGN` | `10.10.20.0/24` | `10.10.20.1` | `10.10.20.100 - .200` | Stations de travail graphistes |
| **VLAN 30** | `PROD-MACHINES` | `10.10.30.0/24` | `10.10.30.1` | IP Statiques pures | Traceurs, Imprimantes industrielles, RIP |
| **VLAN 35** | `PROD-BYOD-STAFF`| `10.10.35.0/24` | `10.10.35.1` | `10.10.35.100 - .250` | PC/Smartphones personnels employés Prod |
| **VLAN 40** | `ADM-CORP` | `10.10.40.0/24` | `10.10.40.1` | `10.10.40.100 - .200` | Direction, Comptabilité, RH, Administratif |
| **VLAN 50** | `SERVERS-INT` | `10.10.50.0/24` | `10.10.50.1` | IP Statiques | AD-DC, DNS, GitLab, TrueNAS, CUPS, GLPI |
| **VLAN 60** | `DMZ-TRANSIT` | `192.168.100.0/29`| N/A (Zone Transit) | IP Statiques | Bastion Guacamole (`192.168.100.2`) |
| **VLAN 70** | `GUEST-NET` | `172.16.10.0/24` | `172.16.10.1` | `172.16.10.10 - .254` | Wi-Fi Invités isolé |
| **VLAN 99** | `MGMT-NET` | `10.10.99.0/24` | `10.10.99.1` | IP Statiques | Interfaces Admin, FW, Switchs |

---

## 3. Adressage des Sites Distants

### Branch 1 — Côte d'Ivoire Annexe (`10.11.0.0/16`)
* **VLAN 10 (DEV) :** `10.11.10.0/24`
* **VLAN 35 (BYOD) :** `10.11.35.0/24`
* **VLAN 40 (ADM) :** `10.11.40.0/24`
* **VLAN 99 (MGMT) :** `10.11.99.0/24`

### Branch 2 — Afrique du Sud (`10.12.0.0/16`)
* **VLAN 10 (DEV) :** `10.12.10.0/24`
* **VLAN 20 (GRAPHIC) :** `10.12.20.0/24`
* **VLAN 40 (ADM) :** `10.12.40.0/24`
* **VLAN 99 (MGMT) :** `10.12.99.0/24`