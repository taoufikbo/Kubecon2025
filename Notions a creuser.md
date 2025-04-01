# Notions à Creuser

## Sommaire
1. [Deep Dive dans eBPF](#deep-dive-dans-ebpf)
   - [Concept Fondamental : eBPF et son Architecture](#1-concept-fondamental-ebpf-et-son-architecture)
   - [Applications et Cas d'Usage de eBPF](#2-applications-et-cas-dusage-de-ebpf)
   - [Types de Programmes eBPF](#3-types-de-programmes-ebpf)
   - [Développement et Outils autour de eBPF](#4-développement-et-outils-autour-de-ebpf)
   - [Sécurité et Vérification](#5-sécurité-et-vérification)
   - [Conclusion](#6-conclusion)

2. [Comparatif entre eBPF et Iptables : Un Use Case Complexe](#comparatif-entre-ebpf-et-iptables-un-use-case-complexe)
   - [Architecture et Flexibilité](#1-architecture-et-flexibilité)
   - [Performance](#2-performance)
   - [Cas d'Usage Complexe : Filtrage et Surveillance des Paquets](#3-cas-dusage-complexe-filtrage-et-surveillance-des-paquets)
   - [Gestion et Maintenance](#4-gestion-et-maintenance)
   - [Conclusion](#5-conclusion)

3. [Deep Dive dans Conntrack avec un Focus sur la Garbage Collection (GC)](#deep-dive-dans-conntrack-avec-un-focus-sur-la-garbage-collection-gc)
   - [Qu'est-ce que Conntrack ?](#1-quest-ce-que-conntrack)
   - [Garbage Collection (GC) dans Conntrack](#2-garbage-collection-gc-dans-conntrack)
   - [Optimisation de la Garbage Collection](#3-optimisation-de-la-garbage-collection)
   - [Problèmes Courants liés à Conntrack GC](#4-problèmes-courants-liés-à-conntrack-gc)
   - [Conclusion](#5-conclusion)

4. [Cilium IPAM Deep Dive avec Comparaison des Alternatives](#cilium-ipam-deep-dive-avec-comparaison-des-alternatives)
   - [Modes d’IPAM supportés par Cilium](#1-modes-dipam-supportés-par-cilium)
   - [Fonctionnement de chaque mode IPAM](#2-fonctionnement-de-chaque-mode-ipam)

5. [Simplifying Multi-Cluster Networking With Cilium and MCS-API](#simplifying-multi-cluster-networking-with-cilium-and-mcs-api)
   - [Introduction à Cilium](#1-introduction-à-cilium)
   - [Introduction à MCS-API](#2-introduction-à-mcs-api)
   - [Intégration de Cilium avec MCS-API](#3-intégration-de-cilium-avec-mcs-api)
   - [Bénéfices de cette approche](#4-bénéfices-de-cette-approche)
   - [Conclusion](#5-conclusion)

6. [Deep Dive : Cluster Mesh et Cilium](#deep-dive-cluster-mesh-et-cilium)
   - [Cluster Mesh](#cluster-mesh)
   - [Cilium](#cilium)
   - [Tableau Comparatif : Cluster Mesh vs Cilium](#tableau-comparatif-cluster-mesh-vs-cilium)
   - [Exemple de Cas d'Utilisation : Secteur des Télécommunications (Telco)](#exemple-de-cas-dutilisation-secteur-des-télécommunications-telco)
   - [Résultats attendus](#résultats-attendus)

7. [Plongée en profondeur dans la passerelle de sortie Cilium](#plongée-en-profondeur-dans-la-passerelle-de-sortie-cilium)
   - [Routage et contrôle du trafic](#1-routage-et-contrôle-du-trafic)
   - [Configuration et politiques](#2-configuration-et-politiques)
   - [Haute disponibilité et évolutivité](#3-haute-disponibilité-et-évolutivité)
   - [Cas d'utilisation](#4-cas-dutilisation)
   - [Considérations de mise en œuvre](#5-considérations-de-mise-en-œuvre)
   - [Dépannage et limitations](#6-dépannage-et-limitations)

---

## Deep Dive dans eBPF

eBPF (extended Berkeley Packet Filter) est une technologie puissante permettant d'exécuter du code dans le noyau Linux de manière flexible, sécurisée et performante. Initialement conçu pour le filtrage de paquets réseau, eBPF a évolué pour être utilisé dans de nombreux domaines, tels que la surveillance, la sécurité, l'optimisation des performances, et plus encore.

### 1. Concept Fondamental : eBPF et son Architecture

eBPF permet d'exécuter du code en espace utilisateur dans le noyau Linux sans nécessiter de modifications du noyau lui-même. Voici comment il fonctionne :

#### Fonctionnement de base
- **BPF Program** : Un programme eBPF est écrit en C (généralement), puis compilé en bytecode. Ce bytecode est chargé dans le noyau Linux pour être exécuté à certains points d'entrée, appelés "hooks".
- **Vérification de sécurité** : Avant l'exécution, le bytecode eBPF est vérifié pour garantir qu'il est sécurisé, pour éviter des problèmes comme les fuites de mémoire ou les boucles infinies.
- **Exécution dans le noyau** : Une fois validé, le programme eBPF peut être exécuté à différents points d'entrée dans le noyau, comme pour filtrer les paquets réseau ou surveiller des événements systèmes.

### 2. Applications et Cas d'Usage de eBPF

Bien qu'eBPF ait été conçu pour filtrer les paquets réseau, il est désormais utilisé dans plusieurs domaines, offrant des possibilités étendues.

#### a. Filtrage réseau (usage initial)
eBPF est utilisé pour créer des filtres réseau ultra-rapides.

#### b. Sécurité (renforcement de la sécurité du noyau)
eBPF permet de renforcer la sécurité du noyau et des applications.

#### c. Monitoring et Performance
- **Observabilité** : eBPF permet une collecte de données fine sur la performance en temps réel.
- **Tracing et Profiling** : Des outils comme **bpftrace** et **perf** utilisent eBPF pour effectuer des tracés dynamiques.

#### d. Manipulation des événements systèmes
eBPF peut capturer et manipuler divers événements du système.

#### e. Networking avancé avec XDP (eXpress Data Path)
XDP permet de traiter des paquets réseau directement à partir de la carte réseau.

### 3. Types de Programmes eBPF

Les programmes eBPF sont classés en fonction de l’endroit où ils peuvent être attachés dans le noyau.

### 4. Développement et Outils autour de eBPF

Le développement de programmes eBPF repose sur l'écriture de code C qui est compilé en bytecode BPF.

#### a. bpftrace
Outil de traçage dynamique.

#### b. BCC (BPF Compiler Collection)
Facilite la création de programmes eBPF.

#### c. Perf
Outil de profilage de performances intégré à Linux.

#### d. Libbpf
Bibliothèque C pour charger et gérer les programmes eBPF.

#### e. XDP-tools
Outils pour gérer les programmes XDP.

### 5. Sécurité et Vérification

Avant l'exécution, les programmes eBPF passent par un processus de vérification pour s’assurer qu'ils sont sans danger.

### 6. Conclusion

eBPF représente une avancée majeure dans la manière dont nous interagissons avec le noyau Linux.

---

## Comparatif entre eBPF et Iptables : Un Use Case Complexe

Iptables et eBPF sont deux technologies populaires pour la gestion du filtrage réseau, mais elles diffèrent largement.

### 1. Architecture et Flexibilité

#### Iptables
- **Architecture** : Fonctionne en analysant les paquets au niveau du noyau.
- **Limitations** : Peut devenir difficile à gérer pour des cas d'utilisation complexes.

#### eBPF
- **Architecture** : Peut interagir avec de nombreux points d’entrée.
- **Limitations** : Nécessite une certaine expertise en programmation C.

### 2. Performance

#### Iptables
- **Inconvénients de performance** : Peut devenir un goulot d'étranglement.

#### eBPF
- **Avantages de performance** : Permet un traitement ultra-rapide des paquets.

### 3. Cas d'Usage Complexe : Filtrage et Surveillance des Paquets

#### Iptables
- **Cas d'usage simple** : Facilité de définition de règles simples.

#### eBPF
- **Cas d'usage avancé** : Permet une grande souplesse et des manipulations complexes.

### 4. Gestion et Maintenance

#### Iptables
- **Complexité de gestion** : Difficile à gérer avec un grand nombre de règles.

#### eBPF
- **Facilité de gestion** : Outils modernes facilitent la gestion.

### 5. Conclusion

| Critère                | **Iptables**                                  | **eBPF**                                          |
|------------------------|-----------------------------------------------|---------------------------------------------------|
| **Architecture**        | Statique, règles définies manuellement        | Dynamique, code personnalisé exécuté dans le noyau |
| **Performance**         | Moins performant avec un grand nombre de règles | Très performant, traitement ultra-rapide avec XDP  |
| **Flexibilité**         | Limitée aux règles de filtrage de paquets     | Très flexible, permet de traiter et manipuler les paquets de manière complexe |
| **Cas d'usage**         | Bon pour des règles simples                   | Idéal pour des cas complexes, comme la surveillance en temps réel ou les DDoS |
| **Facilité de gestion** | Complexe à gérer avec de nombreuses règles    | Facile à gérer avec des outils modernes (bpftrace, BCC, libbpf) |
| **Sécurité**            | Basé sur un ensemble de règles statiques      | Renforce la sécurité avec une vérification de sécurité dynamique du code eBPF |

---

## Deep Dive dans Conntrack avec un Focus sur la Garbage Collection (GC)

**Conntrack** est un sous-système de suivi de connexions du noyau Linux.

### 1. Qu'est-ce que Conntrack ?

Conntrack maintient un tableau de suivi des connexions réseau.

### 2. Garbage Collection (GC) dans Conntrack

La GC est responsable de la gestion de la mémoire utilisée pour stocker les informations sur les connexions.

### 3. Optimisation de la Garbage Collection

Ajuster les valeurs par défaut des délais d'expiration des connexions peut réduire la pression sur la table Conntrack.

### 4. Problèmes Courants liés à Conntrack GC

- **Augmentation du nombre de connexions** : Peut entraîner des ralentissements.
- **Connexions zombies** : Peuvent causer une fuite de mémoire.

### 5. Conclusion

Conntrack est essentiel pour le suivi des connexions réseau, et la gestion de la mémoire par la GC est cruciale pour la performance.

---

## Cilium IPAM Deep Dive avec Comparaison des Alternatives

Cilium prend en charge plusieurs modes d’IPAM.

### 1. Modes d’IPAM supportés par Cilium

| **Mode d’IPAM**         | **Description**                                                                                     | **Scénarios d’utilisation**                   | **Avantages**                                                   | **Inconvénients**                                                 |
|-------------------------|---------------------------------------------------------------------------------------------------|---------------------------------------------|------------------------------------------------------------------|------------------------------------------------------------------|
| **Kubernetes (Cluster) IPAM** | IPAM standard de Kubernetes géré par le composant `kube-controller-manager`.                      | Déploiements Kubernetes classiques.         | - Simple à configurer.  <br> - Compatible avec tous les CNI.    | - Manque de visibilité fine sur les adresses IP. <br> - Moins performant qu’eBPF. |
| **Cilium Native IPAM**   | Géré directement par Cilium. Allocation à partir de pools d’adresses configurables par l’utilisateur. | Environnements où la performance est critique (eBPF).             | - Visibilité avancée sur les adresses IP. <br> - Allocation optimisée. <br> - Compatible avec IPv4 et IPv6. | - Configuration complexe. <br> - Peut nécessiter une personnalisation importante. |
| **AWS IPAM**            | Intégration avec l’API EC2 pour attribuer des **Elastic IPs** ou **Secondary IPs** aux Pods.        | Clusters Kubernetes sur AWS.               | - Allocation automatique via API. <br> - Compatible avec VPC CNI. | - Dépendant de l’infrastructure AWS. <br> - Coût potentiel élevé. |
| **Azure IPAM**          | Intégration avec Azure pour allouer des **Secondary IPs** directement aux Pods.                     | Clusters Kubernetes sur Azure.            | - Utilisation efficace des IPs Azure. <br> - Intégration native. | - Limité aux environnements Azure. <br> - Complexité de configuration. |
| **GCP IPAM**            | Utilisation des **Alias IPs** pour allouer des adresses IP aux Pods.                                | Clusters Kubernetes sur GCP.              | - Bonne intégration avec GCP. <br> - Performances optimisées.   | - Fonctionnalités limitées par rapport à Cilium Native IPAM. |
| **External IPAM**       | API personnalisée pour gérer l’allocation des IPs via une application externe.                     | Environnements hybrides ou spécifiques.   | - Très flexible. <br> - Compatible avec des systèmes tiers.    | - Complexité d’implémentation. <br> - Maintenance potentiellement lourde. |

### 2. Fonctionnement de chaque mode IPAM

#### a. Kubernetes (Cluster) IPAM
- Repose sur les **CIDR pools** configurés au niveau du cluster.

#### b. Cilium Native IPAM
- Géré par Cilium directement via son contrôleur CNI.

---

## Simplifying Multi-Cluster Networking With Cilium and MCS-API

### 1. Introduction à Cilium

Cilium est une plateforme de sécurité et de mise en réseau de niveau 7 (L7).

### 2. Introduction à MCS-API

Le **MCS-API** permet la gestion des services entre plusieurs clusters.

### 3. Intégration de Cilium avec MCS-API

L'intégration de Cilium et de MCS-API offre une solution puissante.

### 4. Bénéfices de cette approche

- **Simplification de la gestion des clusters**.
- **Meilleure sécurité**.

### 5. Conclusion

L'intégration de **Cilium** et de **MCS-API** simplifie la gestion du réseau dans des environnements multi-clusters.

---

## Deep Dive : Cluster Mesh et Cilium

### Cluster Mesh
Le **Cluster Mesh** permet à plusieurs clusters Kubernetes de se connecter entre eux.

### Cilium
**Cilium** est une solution basée sur **eBPF** pour la gestion du réseau.

### Tableau Comparatif : Cluster Mesh vs Cilium

| Critère                        | Cluster Mesh                              | Cilium                                      |
|---------------------------------|-------------------------------------------|---------------------------------------------|
| **Type de technologie**         | Architecture multi-clusters               | Plateforme de gestion réseau basée sur eBPF |
| **Cas d'utilisation principal** | Communication inter-clusters              | Sécurisation et gestion des réseaux         |
| **Performance**                 | Dépend du mécanisme de connectivité       | Haute performance grâce à eBPF              |
| **Sécurité**                    | Connectivité et résilience entre clusters | Filtrage et sécurité du trafic réseau       |
| **Facilité de gestion**         | Centralisation de la gestion des clusters | Visibilité et gestion des politiques réseau |
| **Utilisation de proxy**        | Peut nécessiter un proxy                  | Pas besoin de proxy grâce à eBPF            |

### Exemple de Cas d'Utilisation : Secteur des Télécommunications (Telco)

#### Contexte
Des services doivent être déployés sur plusieurs clusters Kubernetes.

#### Problématique
- **Gestion de l'interconnexion entre plusieurs clusters**.

#### Solution avec Cluster Mesh et Cilium
1. **Cluster Mesh pour l'interconnexion**.
2. **Cilium pour la sécurité du réseau**.

### Résultats attendus
- **Haute disponibilité** des services télécoms.

---

## Plongée en profondeur dans la passerelle de sortie Cilium

### 1. Routage et contrôle du trafic
La passerelle de sortie permet un contrôle précis sur le routage du trafic.

### 2. Configuration et politiques
Les politiques de passerelle de sortie peuvent être configurées pour router le trafic.

### 3. Haute disponibilité et évolutivité
Cilium Enterprise a introduit la haute disponibilité de la passerelle de sortie.

### 4. Cas d'utilisation
Particulièrement utile dans les clusters Kubernetes multi-locataires.

### 5. Considérations de mise en œuvre
Nécessite l'activation du masquage BPF.

### 6. Dépannage et limitations
Une utilisation élevée du mappage des ports SNAT peut entraîner des échecs de connexion.

En résumé, la passerelle de sortie Cilium offre une solution puissante pour gérer le trafic sortant dans les clusters Kubernetes.
