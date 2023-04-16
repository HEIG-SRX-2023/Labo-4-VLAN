# HEIGVD - Sécurité des Réseaux - 2023
# Laboratoire n°4 - VLAN

Ce travail est à réaliser en équipes de deux personnes.
Pour des raisons techniques, les étudiants avec un Mac/Arm (Apple silicon) doivent se joindre à une personne
avec un ordinateur Intel/AMD.
Ceci est dû au fait que l'image que nous utilisons n'est disponible que sous VMWare Intel.

Même si ce labo ne sera pas noté, le contenu pourra faire l'objet de questions lors du prochain TE et de l'examen final.

Vous pouvez répondre aux questions en modifiant directement le README.md de votre fork.

Le rendu consiste simplement à compléter toutes les parties marquées avec la mention "LIVRABLE". Le rendu doit se faire par un `git commit` sur la branche `main`.

## Table de matières

[Introduction](#introduction)

# Introduction

Ce labo vous présente les tagged VLANs à l'exemple d'un système mis en place avec du
matériel CISCO.
Vous avez besoin de l'environnement EVE du cours RXI 2020.
Malheureusement ceci ne tourne pas sans modifications sur les Macs avec apple silicon.
Vous devez donc vous mettre à deux de telle façon qu'il y ait au moins une personne qui a
un ordinateur Intel/AMD.

Avec EVE vous avez la possibilité de voir les différents composants avec leurs connexions,
ainsi que de lancer un Wireshark sur les connexions.
Il est important de suivre les instructions dans le _Manuel_EVE_ pour bien installer
l'environnement.

## Installation

Si vous n'avez pas encore un environnement EVE-NG qui tourne chez vous, suivez les
instructions dans le fichier [Manuel EVE-NG](Manuel_EVE-NG-2.pdf).
Quelques rappels importants:

- vous avez besoin d'un ordinateur avec un processeur Intel/AMD
- n'essayez pas d'installer avec UTM ou autre émulateur, vous n'avez que 2h!
- il faut absolument installer les extensions EVE pour que ça marche
- chez moi je n'ai pas réussi à configurer Chrome pour lancer les applications,
alors j'ai utilisé Firefox et j'ai choisi `EVE Client Tool` pour chaque action
- prenez le temps de parcourir le Manuel, il contient de nombreuses informations
très utiles!
- le nom et mot de passe pour les PCs est `root` - `root` 

## Importer le labo

Suivez les instructions dans le manuel pour importer le `Labo_VLAN-0.3.zip`.
Après vous pouvez ouvrir le labo et démarrer tous les composants.

## Le labo

Vous allez faire ces trois choses pendant le labo:

1. Observer les paquets dans la configuration donnée
2. Modifier la configuration
3. Attaquer le système

## Gestion de fichiers de configuration

Pour chaque partie du labo, commencez par dupliquer le `lab` et donnez-lui un
nom comme `vlan-partie-1`, ou attachez encore un `-bonus`.
Comme ça vous pouvez facilement vous retrouver et refaire des parties de labos.
Tous ces fichiers sont à intégrer dans votre répertoire.

# Observation des paquets

Pour commencer, vous devez observer les paquets entre PC1 et PC2.
Commencez par ouvrir deux wiresharks:
- une fois sur le port e0/0 du SW1
- une fois sur le port e0/0 du Router

Maintenant, vous pouvez ouvrir le PC1 et PC2, afficher l'adresse IP du PC2,
puis lancer un `ping` depuis le PC1 sur le PC2.

Regardez dans les deux fenêtres de Wireshark les pings et répondez aux questions
suivantes:

**Question 1: Quels paquets de ping sont dans un VLAN? Et dans lequel?**

---
**Réponse :**
---

**Question 2: Pourquoi les paquets captés au routeur sont doublés?**

---
**Réponse :**
---

**Question 3: Suivez un paquet de ping depuis le PC1 au PC2 et de retour au PC1 et écrivez**
**pour chaque passage dans un des wireshark sa direction, quel VLAN est utilisé et pourquoi**

---
**Réponse :**
---

# Reconfiguration du système

Maintenant, configurez le système pour qu'il apparaisse comme dans le cours avec les réseaux
`Admin`, `Profs`, et `Etudiants`.
Pour simplifier, vous pouvez enchaîner le troisième switch sur le deuxième switch.
Configurez le routeur afin qu'ils n'accepte que les communications suivantes:

- VLAN 11 et VLAN 12
- VLAN 12 et VLAN 13

Donc le VLAN 13 (étudiants) ne peut pas contacter le VLAN 11 (admin).

**Question 4: Montrez que l'ordinateur du VLAN 13 ne peut pas faire de ping sur le VLAN 11.**
**Attachez une capture d'écran d'un ping avec le timeout.**

---
**Réponse :**
---

**Question 5: Configuration en trois bâtiments - faites une capture d'écran de votre**
**configuration.** 

---
**Réponse :**
---

## Bonus: plus réaliste

Si vous êtes très rapide, vous pouvez aussi reconfigurer le routeur pour avoir un
bridge sur tous ses ports de sortis. Après vous pouvez connecter chaque switch avec un
port individuel du routeur.

**Question 6: Configuration plus réaliste - faites une capture d'écran de votre**
**configuration.**

---
**Réponse :**
---

# Attaquer le système

Maintenant, on va simuler une attaque d'un étudiant qui veut contacter le VLAN 11.

## Configuration

Pour cette attaque, il faut d'abord mal configurer un des ports d'un switch: 
enlevez un `switchport mode access` sur un des ports où un ordinateur pour le VLAN 13
est connecté.

N'oubliez pas de redémarrer le système pour prendre en compte les changements.

## Attaque

Pour simplifier l'attaque, on suppose que l'étudiant a un switch à sa disposition.
Ceci simplifie l'attaque, parce qu'on peut directement connecter le switch au port
mal configuré du VLAN 13.

Donc:
- créez un nouveau switch avec la même configuration que les autres switch
- connectez le switch au port VLAN 13 du switch mal configuré
- configurez une des sorties pour le VLAN 11
- attachez un ordinateur à cette sortie

**Question 7: Faites une capture d'écran de votre nouvelle configuration et attachez-la ici.**

---
**Réponse :**
---

**Question 8: Montrez que l'ordinateur attaché au nouveau switch reçoit une adresse IP**
**du VLAN 11 et que vous pouvez faire un ping sur une autre machine du VLAN 11.**
**Attachez une capture d'écran de `ip a`, suivi d'un `ping` sur vers autre machine.**

---
**Réponse :**
---

## Sécurisation du système

Avec le même système en place, réactivez la ligne `switchport mode access` du port
VLAN 13 et redémarrez le système.
Maintenant l'ordinateur derrière le nouveau switch ne devrait plus recevoir d'adresse
IP du VLAN 11.

**Question 9: Montrez l'ordinateur en question et qu'il ne reçoit pas d'adresse IP.**
**Pour être sûr que c'est bon, attendez au moins 2 minutes.**
**Dans votre capture d'écran, il faut avoir la commande `uptime` qui montre que c'est**
**2 minutes, ainsi que `ip a` qui montre qu'il n'y ait pas d'adresse IP.**

---
**Réponse :**
---

## Bonus: attaque sans switch supplémentaire

Il est possible (mais je ne l'ai pas testé) d'utiliser l'outil `nmcli` sur une machine
linux pour se passer d'un switch.
Donc si vous êtes partant pour un exercice supplémentaire, il faut:
- configurer le routeur pour accéder à internet
- enlever le switch supplémentaire de l'exercice précédent
- installer `nmcli` sur le PC du VLAN 13
- configurer sa connexion réseau pour fonctionner en tant que `trunk`

**Question 10: Quelle est la configuration nécessaire avec nmcli?**
**Écrivez les commandes nécessaires pour configurer le trunk, l'accès au VLAN 11,**
**ainsi que le dhcp qui fournit une adresse du VLAN 11.**

---
**Réponse :**
---
