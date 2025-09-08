# TP 2 – Linux Sécurité & Attaque SSH (Semaine-02)

## 1. Objectif
- Créer un utilisateur sans privilèges (`testuser`)
- Mener une attaque brute-force SSH avec Hydra
- Analyser les logs système liés à `sshd`
- Identifier des mesures de défense simples (limitation des tentatives, Fail2Ban)

---

## 2. Contexte de l’environnement
- VM cible (VM2 : 192.168.100.11) en réseau interne, sans accès Internet  
- Wordlists disponibles localement : `~/SecLists/Passwords/Leaked-Databases/`

---

## 3. Attaque brute-force avec Hydra

```bash
hydra -l vm2 -P ~/SecLists/Passwords/Leaked-Databases/rockyou-75.txt ssh://192.168.100.11
```

### Observation de la progression

```
[STATUS] 122.00 tries/min, 122 tries in 00:01h, 59066 to do in 08:05h, 13 active
```

- Environ **122 essais par minute** avec **13 threads actifs**, estimation de **~8 minutes restantes**.  
- Possibilité de réguler la vitesse de l’attaque avec `-t`, par exemple `-t 4` pour limiter la simultanéité — utile pour éviter d’être détecté ou bloqué. ([turn0search5])

---

## 4. Analyse des logs SSH (`/var/log/auth.log`)

Pendant l’attaque, les lignes suivantes ont été observées :

```
PAM 5 more authentication failures; logname= uid=0 euid=0  
PAM service(sshd) ignoring max retries; 6 > 3  
Failed password for vm2 from 192.168.100.10 port XXXX  
error: maximum authentication attempts exceeded for vm2 192.168.100.10
```

### Explications techniques

- `PAM service(sshd) ignoring max retries; 6 > 3` → PAM est configuré avec `retry=3`, mais SSH permet jusqu’à `MaxAuthTries` (par défaut 6), d’où le message. ([turn0search0], [turn0search2])  
- `maximum authentication attempts exceeded` → SSH interrompt la connexion après avoir atteint cette limite. ([turn0search1], [turn0search11])

---

## 5. Mesures de sécurisation recommandées

### a) Aligner les politiques PAM et SSH

Ajouter dans `/etc/ssh/sshd_config` :

```
MaxAuthTries 3
```

Cela renforce la cohérence et la sécurité du système. ([turn0search0], [turn0search2])

### b) Déployer Fail2Ban

Fail2Ban peut :
- Surveiller les logs (`auth.log`)
- Bloquer automatiquement les IP en cas d’échecs répétés via `iptables`
- Être une défense efficace et automatisée contre les attaques prolongées. ([turn0search14], [turn0search15])

---

## 6. Récapitulatif des actions réalisées

-  Création d’un utilisateur non-administrateur (`testuser`)  
-  Lancement d’une attaque brute-force Hydra mesurée  
-  Analyse des logs d’authentification SSH/PAM  
-  Propositions concrètes : limiter `MaxAuthTries`, déployer Fail2Ban

---

✍️ **Auteur :** Bastien Ladra  
📅 **Date :** Septembre 2025
