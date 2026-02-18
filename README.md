# 🧩 TP VLAN & Masques — Comprendre la segmentation réseau

> Atelier pratique pour comprendre le rôle des VLAN et des masques IP dans la segmentation d’un réseau.

---

# 🎯 Objectifs pédagogiques

À la fin de ce TP, vous devrez savoir :

✅ Expliquer le rôle d’un VLAN  
✅ Comprendre le lien VLAN ↔ sous-réseau IP  
✅ Calculer et utiliser des masques IP  
✅ Mettre en place un trunk 802.1Q  
✅ Tester la communication intra/inter-VLAN

---

# 🧠 Rappel théorique simple

## 📌 VLAN
Un VLAN est un **réseau logique** sur un même switch physique.

👉 Chaque VLAN = un domaine de broadcast distinct  
👉 Chaque VLAN = un sous-réseau IP différent

---

## 📌 Masque de sous-réseau
Le masque définit :
- La partie réseau  
- La partie hôte  

| Masque | Nb hôtes |
|------|---------|
   /24 | 254 |
   /25 | 126 |
   /26 | 62 |

---

# 🗺️ Topologie du TP

```
         VLAN 10         VLAN 20
        192.168.10.0/24 192.168.20.0/24

PC1 -------- SW1 -------- R1 -------- PC3
              |  (trunk)
              |
             PC2
```

---

# 🧩 Matériel Packet Tracer

- 1 routeur (2911)  
- 1 switch (2960)  
- 3 PC  

---

# 🌐 Plan d’adressage

## VLAN 10

| Équipement | IP |
|----------|------|
PC1 | 192.168.10.10/24 |
PC2 | 192.168.10.20/24 |
GW | 192.168.10.1 |

---

## VLAN 20

| Équipement | IP |
|----------|------|
PC3 | 192.168.20.10/24 |
GW | 192.168.20.1 |

---

# 🔹 PARTIE 1 — Création des VLAN

Sur le switch :

```
enable
conf t
vlan 10
name ADMIN
vlan 20
name USERS
```

---

# 🔹 PARTIE 2 — Affectation des ports

```
interface f0/1
switchport mode access
switchport access vlan 10

interface f0/2
switchport mode access
switchport access vlan 10

interface f0/3
switchport mode access
switchport access vlan 20
```

---

# 🔹 PARTIE 3 — Trunk vers le routeur

```
interface f0/24
switchport mode trunk
```

---

# 🔹 PARTIE 4 — Router-on-a-stick

Sur R1 :

```
interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0

interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0

interface g0/0
no shutdown
```

---

# 🔹 PARTIE 5 — Configuration IP des PC

Configurer IP + passerelle selon le plan d’adressage.

---

# 🧪 TESTS

## Test 1 — Intra-VLAN
PC1 → PC2  
👉 Doit fonctionner
 
<img width="1920" height="1040" alt="screenshot_162" src="https://github.com/user-attachments/assets/e4ffc1dd-dc56-4760-8d80-0620f847e028" />

---

## Test 2 — Inter-VLAN
PC1 → PC3  
👉 Fonctionne uniquement grâce au routeur

  <img width="1920" height="1040" alt="screenshot_163" src="https://github.com/user-attachments/assets/b0856167-a77f-48cc-81a3-307b1051a154" />

---

# ❓ Questions de réflexion

1. Pourquoi PC1 ne voit-il pas PC3 sans routeur ? -> Répondez directement sur ce Readme.md 
2. Quel rôle joue le masque /24 ? -> Répondez directement sur ce Readme.md  
3. Que se passe-t-il si VLAN 10 et VLAN 20 ont le même réseau IP ? -> Répondez directement sur ce Readme.md  
4. Pourquoi un trunk est-il nécessaire ? -> Répondez directement sur ce Readme.md

---

# ⭐ Travail sur les Masques

Changer VLAN 10 en :

```
192.168.10.0/25
```

Questions :
- Combien d’hôtes max ?  
- Quelle plage IP valide ?  
- Peut-on encore communiquer avec VLAN 20 ?

---

# 🚀 Extensions

- Ajouter VLAN 30  
- Mettre un DHCP par VLAN  
  
---

# 📝 Évaluation (/20)

| Critère | Points |
|--------|-------|
VLAN créés correctement | 4 |
Ports bien affectés | 2 |
Trunk opérationnel | 4 |
Inter-VLAN fonctionnel | 4 |
Travail sur les masques | 4 |  
Extention | 2 |  
  
# ✅ Fin du TP

Si vous savez expliquer :
> "Pourquoi deux VLAN ne communiquent pas sans routeur ?"

Alors vous avez compris la segmentation réseau 👍
