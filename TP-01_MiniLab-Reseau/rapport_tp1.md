# TP 1 – Mini-lab Réseau (Semaine-01)

## Objectif
Comprendre la configuration réseau de deux VM et tester la réaction d’un firewall basique (UFW) sur le trafic ICMP (ping).

## 1. Contexte
Deux machines virtuelles Ubuntu Server ont été configurées dans un réseau interne. L’objectif était de définir des IP statiques, tester la connectivité réseau (ping), puis sécuriser la communication avec un firewall basique (UFW).

---

## 2. Méthodologie

- **Configuration IP**  
  - VM1 : `192.168.100.10/24`  
  - VM2 : `192.168.100.11/24`  
  - Réseau interne VirtualBox

- **Test initial de connectivité**  
  ```bash
  ping -c 4 192.168.100.11
  ```

Résultat : ping réussi → la communication réseau est fonctionnelle.
Installation et activation du firewall

Sur VM2 :

sudo apt update && sudo apt install ufw -y
sudo ufw enable
sudo ufw deny icmp

Test après durcissement

```bash
ping -c 4 192.168.100.11
```

Résultat : ping bloqué → UFW filtre correctement les paquets ICMP.

## 3. Résultats

La configuration IP fixe et le réseau interne facilitent la communication entre les VM.
L'activation du firewall (blocage de l'ICMP) interdit la réponse au ping, démontrant le contrôle granulaire sur le trafic.

## 4. Conclusion

Ce TP a permis de :

Comprendre la configuration de réseau local entre VM.
Tester l'impact d’un firewall simple sur la connectivité.
Appliquer une première mesure de sécurité réseau (filtrage ICMP).

---

✍️ **Auteur :** Bastien Ladra  
📅 **Date :** Septembre 2025
