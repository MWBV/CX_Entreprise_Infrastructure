# Matrice de Sécurité & Politiques de Filtrage (Zero-Trust)

Ce document établit les règles de filtrage appliquées sur le pare-feu interne `FortiGate-Core` et le pare-feu périmétrique `FortiGate-Edge`. Tout trafic non explicitement autorisé est bloqué (*Deny All par défaut*).

---

## 1. Matrice des Flux Inter-VLANs (FortiGate-Core)

| ID | Source | Destination | Services / Ports | Action | Justification Métier |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **R01** | `VLAN 10 (DEV)` | `VLAN 50 (SERVERS)` | TCP/22 (SSH), 443 (GitLab), 53 (DNS) | **ALLOW** | Accès aux dépôts de code et CI/CD. |
| **R02** | `VLAN 10 (DEV)` | `VLAN 40 (ADM)` | Tout | **DENY** | Isolation réseau de développement vs Bureaux. |
| **R03** | `VLAN 20 (GRAPH)`| `VLAN 50 (SERVERS)` | TCP/445 (SMB), 2049 (NFS) | **ALLOW** | Stockage/partage des fichiers lourds sur NAS. |
| **R04** | `VLAN 20 (GRAPH)`| `VLAN 30 (PRINT)` | TCP/631 (IPP), 9100 (Raw Print) | **ALLOW** | Envoi direct des épreuves sur traceurs. |
| **R05** | `VLAN 30 (PRINT)`| `ANY` | Tout | **DENY** | **Machines de prod isolées d'Internet et LAN.** |
| **R06** | `VLAN 50 (CUPS)` | `VLAN 30 (PRINT)` | TCP/631, 9100 | **ALLOW** | Le serveur CUPS est le seul à spouler la prod. |
| **R07** | `VLAN 35 (BYOD)` | `INTERNET` | TCP/80, 443 (HTTP/HTTPS) | **ALLOW** | Accès Internet filtré (Antivirus/Web Filter). |
| **R08** | `VLAN 35 (BYOD)` | `VLAN 30 (PRINT)` | Tout | **DENY** | **Blocage strict PC perso -> Imprimantes Prod.** |
| **R09** | `VLAN 35 (BYOD)` | `VLAN 50 (SRV)` | TCP/80, 443 (Helpdesk GLPI) | **ALLOW** | Ouverture de tickets de support. |
| **R10** | `VLAN 70 (GUEST)`| `ANY` | TCP/80, 443 | **ALLOW** | Internet uniquement. Isolation de port activée. |

---

## 2. Politiques Périmétriques (FortiGate-Edge)

* **Flux Sortants ADM / DEV / BYOD :** NAT dynamique (PAT) vers les adresses IP publiques des liaisons `ISP-1` / `ISP-2`.
* **Filtrage Web (Web Filtering) :**
  * Catégories bloquées sur tous les VLANs : *Malware, Phishing, Adult, P2P/Torrent, Gambling*.
  * Inspection SSL/TLS active sur les flux HTTPS.
* **Prévention des Intrusions (IPS) :** Analyse appliquée sur tous les flux transitant par la DMZ Transit.