# 🔐 Zone de Transit & Flux d'Accès Distant (Bastion Guacamole)

Ce document décrit le processus sécurisé permettant à un intervenant extérieur (prestataire, technicien de maintenance d'imprimante) d'accéder aux équipements de production sans exposer le réseau interne à Internet.

---

## 1. Séquence d'Accès Sécurisé

```text
[Utilisateur Externe]
        |
        | 1. Connexion VPN SSL (MFA)
        v
[FortiGate-Edge]
        |
        | 2. Redirection stricte en DMZ Transit (192.168.100.0/29)
        v
[Bastion Guacamole (192.168.100.2)]
        |
        | 3. Authentification Web HTML5 + Choix de la cible
        v
[FortiGate-Core]
        |
        | 4. Inspection de la règle R_BASTION (SSH/RDP filtré)
        v
[Cible en Production (VLAN 10 / VLAN 30)]