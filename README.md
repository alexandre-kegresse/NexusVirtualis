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

* **Système :** Debian 12 (Bookworm).
* **Spécifications (respect des contraintes) :**
    * CPU : 2 vCPU.
    * RAM : 1 Go (1024 Mo).
    * Disque : 8 Go.
    * Interface : CLI (Command Line Interface - Sans GUI).
* **Réseau :** Connecté au vSwitch par défaut (Accès Internet via NAT).
  <img width="1920" height="1020" alt="Capture d&#39;écran 2026-01-19 145623" src="https://github.com/user-attachments/assets/79a8c9ea-0ae0-4938-93bb-817ef2156ee0" />

