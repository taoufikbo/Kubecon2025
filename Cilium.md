# Intervenants : 
https://www.luigizhou.com/

Neha Aggarwal
Principal Software Engineer Manager, Microsoft

Liz Rice
Chief Open Source Officer, Isovalent at Cisco


# 🌐 Cilium pour les Déploiements Telco sur OpenShift

Cilium utilise **eBPF** pour offrir des fonctionnalités avancées de réseau, de sécurité, et d'observabilité au sein de clusters Kubernetes comme OpenShift. Pour les Telcos, cela est essentiel pour des workloads critiques comme **5G Core, CNFs, et Edge Computing**.

---

## 🚀 1. Networking Optimisé avec eBPF

Cilium remplace les composants réseau traditionnels (ex. `iptables`, `IPVS`) par une implémentation entièrement basée sur **eBPF** :

- **Load Balancing natif** au niveau du kernel, offrant de meilleures performances.
- **Routage avancé** via eBPF XDP pour un traitement ultra-rapide, idéal pour l’UPF (User Plane Function) en 5G.
- **Support de SRv6** : Routage sophistiqué pour le **Network Slicing** dans les déploiements 5G.

### 📌 Exemple
- Communication efficace entre composants **Core 5G** (AMF, SMF, UPF) grâce au routage L3/L4 rapide entre pods.

---

## 🔒 2. Sécurité Avancée avec eBPF

- **Politiques réseau granulaires (L3/L4/L7)** basées sur des identités (ID de pods) au lieu d’IP statiques.
- **Inspection profonde des paquets (DPI)** via eBPF pour appliquer des politiques basées sur des protocoles applicatifs.
- **Sécurité Zero-Trust** : Authentification basée sur l'identité, non sur l’IP.

### 📌 Exemple
- Contrôle de l’accès aux APIs spécifiques du Core 5G (par exemple, restreindre l’accès à l’interface N11 entre AMF et SMF).

---

## 🔍 3. Observabilité et Troubleshooting

- **Cilium Hubble** : Visualisation en temps réel du trafic réseau.
- **Traçabilité L7 complète** : Permet de diagnostiquer les problèmes de communication entre composants réseau.
- **Metrics détaillées** : Monitoring de la latence, du throughput, des erreurs, etc.

### 📌 Exemple
- Visualisation des appels HTTP entre **AMF, SMF, UPF** avec des métriques détaillées.

---

## 🌐 4. Service Mesh sans Sidecar

- Fonctionne sans sidecar grâce à des hooks eBPF au niveau du kernel.
- **Réduction drastique de la latence** pour les workloads Telco critiques comme l’UPF.
- Application de politiques réseau L7 sans overhead supplémentaire.

### 📌 Exemple
- Application de politiques réseau basées sur les URI des APIs exposées par les composants 5G.

---

## 🔗 5. Intégration OpenShift et Cilium

Cilium peut remplacer le CNI natif (OpenShift SDN) pour fournir des fonctionnalités réseau avancées via eBPF :

- **Support natif d’OpenShift** : Intégration fluide avec les fonctionnalités Kubernetes standard (NetworkPolicy, Ingress, etc.).
- **Observabilité intégrée** via **Hubble**, directement accessible depuis la console OpenShift.
- **Scalabilité améliorée** pour des clusters massifs sur VM ou bare metal.

---

## 📈 Comparatif Cilium, Calico, et OVN-Kubernetes

| Feature              | **Cilium (eBPF)**  | **Calico (iptables/eBPF)** | **OVN-Kubernetes (OVS/OVN)** |
|----------------------|-------------------|----------------------------|------------------------------|
| Data Plane           | eBPF              | iptables / eBPF            | Open vSwitch (OVS)           |
| Performance          | 🔥 Très élevée (eBPF natif) | Moyenne (iptables) / Haute (eBPF) | Moyenne à élevée             |
| Security Policies    | L3/L4/L7 (eBPF)   | L3/L4 (iptables/eBPF)      | L3/L4 (OVS)                  |
| Observability        | Intégré avec Hubble (eBPF) | Basique (Flow logs)       | Limité (OVN tracing)         |
| Service Mesh         | Sidecar-less (eBPF) | Avec sidecars (Istio)      | Non supporté                |
| Scalability          | Très élevée       | Moyenne à élevée           | Moyenne                     |
| Telco Use Cases      | Excellent         | Bon                        | Limité                      |

---

## 📈 Comparatif eBPF vs. IPTables

| Feature            | **eBPF**             | **IPTables**               |
|--------------------|---------------------|---------------------------|
| Performance        | 🔥 Très élevée (kernel-space) | Moyenne (user-space)     |
| Scalabilité        | Très élevée          | Moyenne                   |
| Latence            | Faible               | Élevée (table traversal)  |
| Sécurité           | Granulaire (L3/L4/L7) | Basique (L3/L4 uniquement) |
| Observabilité      | Excellente (Hubble, BPF programs) | Limitée (Logs uniquement) |
| Maintenance        | Dynamique (sans recompilation) | Statique (reload nécessaire) |
| Overhead           | Faible               | Élevé                     |

---

## 🖼️ Schéma Illustratif

Voici un diagramme montrant comment **Cilium utilise eBPF pour fournir des fonctionnalités avancées de réseau, sécurité, et observabilité** dans un environnement Telco sur OpenShift.

![Cilium Telco Diagram](./A_schematic_diagram_illustrates_how_Cilium_utilize.png)

---

## 📚 Références

- [Documentation Cilium](https://cilium.io/docs/)
- [Hubble Observability](https://docs.cilium.io/en/stable/gettingstarted/hubble/)

# ✅ Pourquoi Choisir Cilium pour les Déploiements Telco sur OpenShift

Cilium est devenu un choix privilégié pour les déploiements Cloud-Native, en particulier pour les infrastructures Telco exigeantes. Voici pourquoi :

---

## 🚀 **1. Performances Exceptionnelles grâce à eBPF**

- **Traitement Kernel-Space :** Contrairement aux approches traditionnelles basées sur `iptables`, Cilium utilise eBPF pour exécuter des programmes directement dans le kernel Linux.
- **Réduction de la Latence :** Pas de passage fréquent entre l'espace utilisateur et l'espace noyau.
- **Optimisation XDP (Express Data Path) :** Traitement ultra-rapide des paquets réseau, essentiel pour les workloads Telco tels que l’UPF (User Plane Function).

---

## 🔒 **2. Sécurité Granulaire et Scalabilité**

- **Politiques Réseau Fines :** Support L3/L4/L7 grâce à eBPF, permettant des règles précises jusqu’au niveau applicatif.
- **Zero-Trust Networking :** Authentification et autorisation basées sur l'identité des workloads plutôt que sur les adresses IP.
- **Scalabilité Horizontale :** Convient aux grands clusters Kubernetes avec des milliers de pods.

---

## 🔍 **3. Observabilité Intégrée (Hubble)**

- **Traçabilité Complète :** Visualisation en temps réel des flux réseau entre workloads.
- **Analyse L7 :** Détection et suivi des requêtes HTTP, gRPC, DNS, etc.
- **Troubleshooting Amélioré :** Outil puissant pour diagnostiquer les problèmes de connectivité au niveau réseau.

---

## 🌐 **4. Service Mesh sans Sidecar (eBPF-native)**

- **Réduction de l'Overhead :** Pas besoin d’injecter des sidecars comme avec Istio, ce qui améliore les performances.
- **Politiques L7 sans Proxy :** Grâce à eBPF, les politiques applicatives peuvent être appliquées directement au niveau du kernel.
- **Meilleure Résilience :** Moins de composants introduits réduit les risques de panne.

---

## 🔧 **5. Intégration Optimale avec OpenShift**

- **CNI (Container Network Interface) Remplaçable :** Cilium peut remplacer le SDN par défaut d’OpenShift pour apporter plus de fonctionnalités avancées.
- **Compatibilité Complète :** Fonctionne sur OpenShift qu'il soit déployé sur **VMs ou Bare Metal**.
- **Observabilité Accessible via la Console OpenShift :** Grâce à l’intégration de **Hubble**.

---

## 📊 **6. Comparaison avec d'autres Solutions (Calico, OVN-Kubernetes)**

| Feature              | **Cilium (eBPF)**  | **Calico (iptables/eBPF)** | **OVN-Kubernetes (OVS/OVN)** |
|----------------------|-------------------|----------------------------|------------------------------|
| Data Plane           | eBPF              | iptables / eBPF            | Open vSwitch (OVS)           |
| Performance          | 🔥 Très élevée (eBPF natif) | Moyenne (iptables) / Haute (eBPF) | Moyenne à élevée             |
| Security Policies    | L3/L4/L7 (eBPF)   | L3/L4 (iptables/eBPF)      | L3/L4 (OVS)                  |
| Observabilité        | Intégré avec Hubble (eBPF) | Basique (Flow logs)       | Limité (OVN tracing)         |
| Service Mesh         | Sidecar-less (eBPF) | Avec sidecars (Istio)      | Non supporté                |
| Scalability          | Très élevée       | Moyenne à élevée           | Moyenne                     |
| Telco Use Cases      | Excellent         | Bon                        | Limité                      |

---

## 🔥 **7. Comparatif eBPF vs. IPTables**

| Feature            | **eBPF**             | **IPTables**               |
|--------------------|---------------------|---------------------------|
| Performance        | 🔥 Très élevée (kernel-space) | Moyenne (user-space)     |
| Scalabilité        | Très élevée          | Moyenne                   |
| Latence            | Faible               | Élevée (table traversal)  |
| Sécurité           | Granulaire (L3/L4/L7) | Basique (L3/L4 uniquement) |
| Observabilité      | Excellente (Hubble, BPF programs) | Limitée (Logs uniquement) |
| Maintenance        | Dynamique (sans recompilation) | Statique (reload nécessaire) |
| Overhead           | Faible               | Élevé                     |

---

## 📌 **Conclusion**

Cilium se démarque par sa capacité à tirer parti d’eBPF pour améliorer les performances, la sécurité et l’observabilité dans des environnements Kubernetes complexes comme OpenShift. Il est particulièrement adapté aux déploiements Telco où la **latence faible**, la **scalabilité élevée**, et une **visibilité fine** sont des critères cruciaux. 

---



---

