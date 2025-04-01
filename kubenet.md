# Kubenet Deep Dive

## Introduction
Kubenet est un **plugin réseau simple** utilisé par Kubernetes pour gérer la communication entre les pods. Bien qu'il soit limité par rapport aux plugins CNI avancés comme Calico, Cilium ou NSX, il reste couramment utilisé dans des environnements de développement ou de petite envergure.

---

## Fonctionnement de Kubenet

### 🔑 **Architecture**
- Kubenet fonctionne en **utilisant les interfaces réseau de l'hôte** pour connecter les pods entre eux.
- Il repose principalement sur l’utilisation de **bridge networking** pour la communication intra-nœud et **route tables** pour la communication inter-nœud.

### 📦 **Composants principaux**
1. **Bridge (cbr0)** :
   - Un bridge virtuel créé sur chaque nœud pour connecter les pods localement.
   - Chaque pod se voit attribuer une IP provenant d’un sous-réseau propre au nœud.

2. **iptables** :
   - Utilisé pour implémenter des règles de filtrage et de NAT afin de permettre la communication entre les pods.

3. **Route Tables** :
   - Kubenet configure des routes pour permettre aux nœuds de communiquer entre eux en utilisant les adresses IP des pods.

4. **kubelet** :
   - Configure l’interface réseau en fonction de la configuration définie pour le cluster Kubernetes.

---

## Limites de Kubenet

- ❌ **Pas de support pour NetworkPolicies** : Kubenet ne prend pas en charge les stratégies réseau Kubernetes. Cela limite la sécurité et le contrôle des communications entre les pods.
- ❌ **Scalabilité limitée** : Il est adapté pour des environnements simples mais devient rapidement complexe à gérer pour des clusters de grande envergure.
- ❌ **Pas d'intégration avancée avec CNI** : Contrairement à Calico ou Cilium, Kubenet ne fournit pas de fonctionnalités de sécurité avancées, de routage complexe ou de monitoring intégré.

---

## Use Cases
- Kubenet est principalement utilisé dans :
  - Les **environnements de développement** ou **tests locaux**.
  - Des **clusters simples** où les fonctionnalités avancées de sécurité ne sont pas nécessaires.
  - Des scénarios d'apprentissage ou de POC (Proof Of Concept).

---

## Alternatives
Pour des environnements de production ou nécessitant des fonctionnalités avancées, il est recommandé d'utiliser des plugins CNI tels que :
- **Calico** : Pour des politiques réseau avancées et la sécurité.
- **Cilium** : Pour l’observabilité, le routage L7, et la sécurité eBPF.
- **NSX** : Spécialement pour les infrastructures VMware.

---

## Conclusion
Kubenet est un plugin réseau simple, adapté pour des cas d'utilisation légers, mais limité en termes de sécurité, de scalabilité, et de fonctionnalités avancées. Pour des besoins complexes, il est préférable de se tourner vers des plugins CNI modernes.
