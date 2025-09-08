# Rapport d'Audit de Sécurité – Mini-Projet Réseau (Semaine 1)

## 1. Contexte
Dans le cadre d’un exercice de cybersécurité, un petit réseau composé de deux machines virtuelles a été mis en place afin de tester la communication, la sécurisation des services et la détection d’attaques simples.

## 2. Architecture mise en place
- **Réseau interne** : 192.168.200.0/24  
- **VM1 (Attaquant)** : 192.168.200.10 (Ubuntu)  
- **VM2 (Cible)** : 192.168.200.11 (Ubuntu)  

### Schéma simplifié
```
[ VM1 (Attaquant) ] ---- (192.168.200.0/24) ---- [ VM2 (Cible) ]
```

## 3. Mesures de sécurité appliquées
- Création d’un utilisateur dédié (`securite`) → interdiction du login root.  
- Activation d’UFW sur VM2 :
  - Autorisation SSH uniquement depuis VM1.  
  - Blocage par défaut des autres connexions.  
- Vérification des journaux système via `/var/log/auth.log`.  

## 4. Attaque simulée
- Outil utilisé : **Hydra**.  
- Commande :
  ```bash
  hydra -l securite -P /usr/share/wordlists/rockyou.txt ssh://192.168.200.11

### Résultats observés
- ❌ Plusieurs tentatives détectées dans les logs  
- 🔑 Aucun mot de passe compromis (mot de passe complexe utilisé)  

## 5. Résultats & Observations
- ✅ Le firewall a correctement restreint l’accès au port SSH  
- ✅ Les journaux montrent bien les tentatives d’authentification échouées  
- ✅ Mise en évidence d’un scénario de brute force en conditions réelles  

---
✍️ **Auteur :** Bastien Ladra  
📅 **Date :** Septembre 2025

