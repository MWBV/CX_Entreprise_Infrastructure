# CX_Entreprise_Infrastructure

# 🏢 Enterprise Multi-Site Network Infrastructure & Zero-Trust Architecture — CX

[![Architecture](https://img.shields.io/badge/Architecture-Zero--Trust-blue.svg)]()
[![Firewall](https://img.shields.io/badge/Firewall-Fortinet_FortiGate-red.svg)]()
[![WAN](https://img.shields.io/badge/WAN-FortiGate_Secure_SD--WAN-green.svg)]()
[![Lab Environment](https://img.shields.io/badge/Lab-GNS3_on_Proxmox-orange.svg)]()

## 📌 Présentation du Projet

Ce projet documente et simule la conception complète d'une infrastructure réseau et systèmes d'entreprise pour la société **CX** (spécialisée dans le code/développement, le design graphique et l'impression industrielle).

L'architecture s'articule autour d'un siège principal (**QG en Côte d'Ivoire**) réparti sur deux Data Centers (DC-A & DC-B), d'un site annexe (**Branch 1 - CI**) et d'un site distant d'outre-mer (**Branch 2 - Afrique du Sud**).

---

## 🎯 Points Clés de l'Architecture

* **Double Pare-feu (Edge vs Core) :** Un `FortiGate-Edge` dédié à la bordure Internet/SD-WAN et un `FortiGate-Core` dédié à la segmentation interne et l'inspection inter-VLAN.
* **FortiGate Secure SD-WAN :** Agrégation des liens `ISP-1` et `ISP-2` avec basculement automatique (*SLA Steering*) selon la latence et le gigotage vers la Branch 2.
* **Isolation Strictement Séparée (Zero-Trust) :**
  * **Production (Machines) :** Totalement isolée d'Internet et inaccessible directement depuis les postes clients.
  * **BYOD Staff :** Postes personnels isolés du réseau local d'impression, autorisés uniquement sur Internet et l'Helpdesk.
* **Zone de Transit Sécurisée (DMZ Transit) :** Accès prestataire distant via VPN SSL vers un **Bastion de rebond (Apache Guacamole)** avec contrôle strict des accès RDP/SSH vers la production.

---

## 🏗️ Topologie Réseau Globale

```text
                      [ INTERNET ]
                           |
             +-------------+-------------+
             | ISP-1             ISP-2   |
             +-------------+-------------+
                           |
             +-------------v-------------+
             | FW-INTERNET (FortiGate)   |
             | - Secure SD-WAN           |
             | - Terminaison VPN SSL     |
             +-------------+-------------+
                           |
         === DMZ TRANSIT (192.168.100.0/29) ===
                           |
           +---------------+---------------+
           |                               |
+----------v----------+         +----------v----------+
| Bastion Guacamole   |         | FW-INTRANET         |
| (Accès Extérieur)   |         | (Core FortiGate)    |
+---------------------+         +----------+----------+
                                           |
                                === CŒUR DE RÉSEAU ===
                                           |
  +-------+-------+-------+-------+--------+-------+-------+
  |       |       |       |       |        |       |       |
VLAN10  VLAN20  VLAN30  VLAN35  VLAN40   VLAN50  VLAN60  VLAN70
 (DEV) (GRAPH) (PROD- (BYOD)  (ADM)    (SRV)   (DMZ)   (GUEST)
               PRINT)