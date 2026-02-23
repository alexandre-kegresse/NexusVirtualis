# Job 01 : Concepts de Base de la Virtualisation

## 1. Différence entre Hyperviseurs de Type 1 et Type 2

La distinction principale réside dans leur emplacement par rapport au matériel (Hardware) :

* **Hyperviseur de Type 1 (Bare Metal) :**
    * Il s'installe **directement sur le matériel physique** du serveur.
    * Il fait office de système d'exploitation ultra-léger dédié uniquement à la virtualisation.
    * *Exemples :* VMware ESXi, Microsoft Hyper-V (Server), Proxmox VE, XCP-ng.

* **Hyperviseur de Type 2 (Hosted) :**
    * Il s'installe **comme un logiciel** sur un système d'exploitation hôte déjà présent (Windows, Linux, macOS).
    * Il dépend de l'OS hôte pour gérer les ressources matérielles.
    * *Exemples :* VMware Workstation, Oracle VirtualBox.

## 2. Avantages et Inconvénients du Type 1

| Avantages | Inconvénients |
| :--- | :--- |
| **Performance :** Accès direct au matériel sans couche OS intermédiaire (overhead réduit). | **Compatibilité Matérielle (HCL) :** Nécessite du matériel certifié et spécifique (pilotes limités). |
| **Sécurité :** Surface d'attaque réduite (OS minimaliste). | **Complexité de gestion :** L'administration se fait souvent à distance ou en ligne de commande, moins intuitive pour les débutants. |
| **Stabilité :** Isolation forte entre les VMs et l'hyperviseur. | **Monotâche :** Le serveur physique est dédié à la virtualisation (inutilisable comme poste de travail). |

## 3. Cas d'utilisation typiques
* **Data Centers et Cloud Computing :** Pour la consolidation de serveurs (ex: AWS, Azure, OVH).
* **Entreprises :** Pour héberger des serveurs de production (AD, Web, Base de données) avec une haute disponibilité.
* **VDI (Virtual Desktop Infrastructure) :** Pour virtualiser des postes de travail utilisateurs.

# Job 04 : Hyper-V et Virtualisation Imbriquée

## 1. Installation de l'Hyperviseur (Windows Server 2022)
* **Hyperviseur Hôte :** VMware Workstation Pro.
* **VM créée :** Windows Server 2022 Standard (Desktop Experience).
* **Configuration :** 4 vCPU, 8 Go RAM, Virtualisation Intel VT-x/EPT activée (Nested Virtualization).
* **Installation du Rôle :** Ajout du rôle "Hyper-V" via le Gestionnaire de serveur.

## 2. Création de la VM Invité (Debian)
Nous avons créé une machine virtuelle Debian au sein de l'hyperviseur Hyper-V.

* **Système :** Debian 13.
* **Spécifications (respect des contraintes) :**
    * CPU : 2 vCPU.
    * RAM : 1 Go (1024 Mo).
    * Disque : 8 Go.
    * Interface : CLI (Command Line Interface - Sans GUI).
* **Réseau :** Connecté au vSwitch par défaut (Accès Internet via NAT).
  
  <img width="1920" height="1020" alt="Capture d&#39;écran 2026-01-19 145623" src="https://github.com/user-attachments/assets/79a8c9ea-0ae0-4938-93bb-817ef2156ee0" />

# Job 05 : VMware ESXi

## 1. Installation de l'Hyperviseur
Nous avons déployé VMware ESXi (Type 1) en virtualisation imbriquée.

* **Version :** VMware ESXi 6.7.0
* **Ressources allouées à l'ESXi :** 2 vCPU, 4 Go RAM.
* **Administration :** Via l'interface web (ESXi Host Client).

## 2. Création de la VM Invité (Debian)
Création d'une machine virtuelle Debian hébergée sur l'infrastructure ESXi.

* **Configuration (Conforme aux spécifications) :**
    * CPU : 2 vCPU.
    * RAM : 1024 MB.
    * Disque : 8 Go (Thin Provisioning).
    * OS : Debian (Mode texte / Server only).
* **Méthode d'installation :** Upload de l'ISO dans le Datastore ESXi et montage sur le lecteur virtuel.

<img width="1920" height="1020" alt="Capture d&#39;écran 2026-01-19 155432" src="https://github.com/user-attachments/assets/a694f1d3-fec1-487c-9ce4-6d1bea2a4e8e" />

# Job 06 : Proxmox VE

## 1. Installation de l'Hyperviseur
Proxmox VE est une solution open-source basée sur Debian, combinant virtualisation KVM et conteneurs LXC.

* **Version :** Proxmox VE 9.1.1
* **Ressources allouées :** 2 vCPU, 4 Go RAM.
* **Administration :** Via interface web sur le port 8006.

## 2. Création de la VM Invité (Debian)
Déploiement d'une VM Debian via l'interface web de Proxmox.

* **Configuration :**
    * CPU : 2 Cores (Type KVM64 ou Host).
    * RAM : 1024 MiB.
    * Disque : 8 GiB (Stockage local LVM ou Directory).
    * OS : Debian 13 (Netinstall).
* **Particularité :** Upload de l'ISO dans le stockage "local" avant création.
  
<img width="1920" height="1020" alt="Capture d&#39;écran 2026-01-19 163239" src="https://github.com/user-attachments/assets/07134fef-26e9-4823-b1ee-afd9db20fbad" />

<img width="1920" height="1080" alt="Capture d&#39;écran 2026-01-19 163127" src="https://github.com/user-attachments/assets/3e07a5fa-dc41-4081-bbc5-120363d95fed" />

# Job 07 : XCP-ng

## 1. Installation de l'Hyperviseur
XCP-ng est une distribution de virtualisation basée sur Xen Server (Type 1).

* **Version :** XCP-ng 8.2 (LTS) ou 8.3
* **Ressources :** 2 vCPU, 4 Go RAM.
* **Gestion :** Via le client lourd Windows "XCP-ng Center" (pour éviter le déploiement de Xen Orchestra en nested).

## 2. Création de la VM Invité (Debian)
* **Configuration :**
    * Template : Debian 10/11/12.
    * Ressources : 2 vCPU, 1 Gio RAM, 8 Go Disque.
    * Réseau : Ponté sur l'interface de gestion (eth0).

<img width="1920" height="1020" alt="Capture d&#39;écran 2026-01-20 115744" src="https://github.com/user-attachments/assets/98515367-54a7-4143-96ee-282b2a2be46b" />

# Job 08 : Migration de VMs (Téléportation)

L'objectif est de migrer une machine virtuelle à travers différents hyperviseurs selon le chemin :
**Hyper-V -> ESXi -> Proxmox -> XCP-ng**.

## Étape 1 : Hyper-V vers ESXi
Nous avons utilisé l'outil **StarWind V2V Converter** pour convertir le disque à chaud.

* **Source :** Hyper-V (Format .vhdx)
* **Destination :** ESXi Server (Injection directe dans le Datastore).
* **Format de conversion :** VMDK (ESXi Growable Image).
* **Résultat :** La VM "Debian_Migrated" a démarré avec succès sur ESXi.
  
<img width="1920" height="1080" alt="Capture d&#39;écran 2026-01-20 135358" src="https://github.com/user-attachments/assets/7694881e-b0f3-4521-82d5-33ac7eedcbb7" />

## Étape 2 : ESXi vers Proxmox

Nous avons validé la migration par deux méthodes (flux SSH via `dd` et l'assistant d'importation natif de Proxmox) après avoir dû étendre manuellement la partition système de l'hôte, le stockage de données par défaut (7 Go) étant physiquement plus petit que le disque source (8,6 Go).
* **Source :** ESXi Server (Format .vmdk - Flat Image).
* **Destination :** Proxmox VE (Stockage local sur partition root étendue).
* **Format de conversion :** RAW (Image disque brute).
* **Résultat :** La VM "101 (Debian-Migrated)" a démarré en mode UEFI (Secure Boot désactivé) après extension manuelle du volume LVM.

<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/ca0b2ba8-a72d-468c-9e55-681b05419b3c" />

## Étape 3 : Proxmox VE vers XCP-ng (Analyse des limites et contraintes)

Cette étape finale visait à migrer la VM Debian de Proxmox vers XCP-ng via un transfert de flux (streaming) ou une conversion de disque. En raison de contraintes matérielles liées à l'environnement de laboratoire "Nested", cette étape n'a pas pu être finalisée. Voici l'analyse technique des blocages rencontrés.

### ❌ Problématiques de stockage (Storage Exhaustion)

* **Saturation sur l'hôte Proxmox** : La partition racine (`/`) ne disposait que de 4,2 Go d'espace libre sur 17,98 Go au total.
* **Impossibilité de conversion locale** : Le disque source de la VM pesant 8,6 Go, la création d'un clone au format VHD a systématiquement échoué par manque d'espace disque (Erreur : `No space left on device`).
* **Saturation sur l'hôte XCP-ng** : Le stockage local de destination s'est retrouvé saturé avec seulement 336 Mo de disponible.
* **Erreurs d'importation** : Les tentatives de transfert direct via tunnel SSH ont généré des erreurs de type `VDI_IO_ERROR` dues à l'impossibilité pour l'hôte de destination d'écrire les données sur un support physique plein.

### ⚙️ Complexité de l'architecture de démarrage (UEFI)

* **Multi-Hyperviseurs** : La VM ayant transité par Hyper-V, ESXi puis Proxmox, la persistance de la table de partition GPT et du mode de démarrage UEFI a nécessité des reconfigurations constantes des firmwares virtuels.
* **Contraintes Nested** : L'utilisation d'hyperviseurs imbriqués a limité les performances d'E/S (Entrées/Sorties), rendant les flux de données instables lors des transferts de gros volumes de données.

### 💡 Bilan technique

Bien que la VM n'ait pas pu être démarrée sur XCP-ng, les manipulations effectuées ont permis de valider des compétences critiques :
1.  **Gestion avancée de LVM** : Désactivation du Swap et extension de partitions logiques en ligne de commande pour tenter de libérer de l'espace système.
2.  **Manipulation de flux SSH** : Utilisation de `qemu-img` et `dd` pour tenter de contourner les limitations de stockage par l'utilisation de "pipes" (tuyaux) de données.
3.  **Diagnostic d'infrastructure** : Identification précise des limites du *Capacity Planning* nécessaires à la réussite d'un projet de migration multi-cloud.

## Job 09 : Mise en œuvre de Proxmox Backup Server (PBS)

[cite_start]Afin de sécuriser l'infrastructure virtualisée sous Proxmox VE [cite: 49][cite_start], nous avons déployé et configuré une solution de sauvegarde centralisée avec **Proxmox Backup Server**[cite: 50].

### 🛠️ Configuration de la solution
1. **Installation** : Déploiement de PBS (soit en bare-metal, soit en VM dédiée) et ajout du stockage de sauvegarde dans le cluster Proxmox VE.
2. [cite_start]**Planification des sauvegardes** : Configuration d'un "Backup Job" pour la VM Debian avec une fréquence de **toutes les 2 heures**.
   * *Paramètre Cron* : `0 */2 * * *`
3. [cite_start]**Politique de rétention (Pruning)** : Mise en place d'une règle de nettoyage automatique pour ne conserver que les **3 dernières sauvegardes**.
   * *Paramètre de rétention* : `keep-last=3`

### ✅ Avantages constatés
* **Déduplication à la source** : Réduction massive de l'espace disque utilisé, les sauvegardes étant incrémentales après le premier passage.
* **Vérification d'intégrité** : Possibilité de planifier des vérifications automatiques des données sauvegardées.

---

## Job 10 : Documentation sur les solutions de sauvegarde de VMs

[cite_start]Dans le cadre de cette mission, nous avons étudié les différentes solutions professionnelles permettant la protection des environnements virtualisés[cite: 54].

### 📊 Comparatif des solutions majeures

| Solution | Hyperviseurs supportés | Points forts |
| :--- | :--- | :--- |
| **Proxmox Backup Server** | Proxmox VE | [cite_start]Intégration native, déduplication ultra-performante, Open Source[cite: 73]. |
| **Veeam Backup & Replication** | ESXi, Hyper-V | [cite_start]Standard du marché, restauration granulaire (fichiers/SQL), support multi-cloud[cite: 70, 74]. |
| **Xen Orchestra (XO)** | XCP-ng, XenServer | [cite_start]Sauvegardes Delta, réplication continue, interface Web intégrée[cite: 71]. |
| **Nakivo Backup** | ESXi, Hyper-V, Proxmox | Solution légère, installation sur NAS (Synology/QNAP), coût attractif. |

### 🛡️ Concepts clés de la sauvegarde virtuelle
* **Snapshots vs Backups** : Un snapshot n'est pas une sauvegarde ; il dépend du disque source, contrairement au backup exporté sur un stockage tiers.
* **3-2-1 Rule** : 3 copies des données, sur 2 supports différents, avec 1 copie hors-site (Offsite).
* **RPO / RTO** : Définition de la perte de données maximale admissible (RPO) et du temps de rétablissement (RTO). [cite_start]Dans le Job 09, le RPO est fixé à 2 heures.
