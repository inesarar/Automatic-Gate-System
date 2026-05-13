# Automatic Gate System (AGS)

## Présentation du projet
Le projet **Automatic Gate System (AGS)** consiste à réaliser un système de barrière automatique intelligente capable de détecter un véhicule, de transmettre l’information via une communication LoRa, puis de commander l’ouverture automatique d’une barrière.

Le projet repose sur :
- une carte UCA émettrice ;
- une carte UCA réceptrice ;
- un capteur ultrason HC-SR04 ;
- un servomoteur ;
- une communication radio LoRa 868 MHz ;
- une maquette de parking à 4 places ;
- un futur affichage OLED et système de comptage des places.

---

# Répartition des rôles

## Personne 1 — Maquette et partie physique (mon rôle)

Responsabilités :
- construction de la maquette du parking ;
- réalisation de la route et des places ;
- fabrication de la barrière ;
- intégration mécanique du servomoteur ;
- montage physique global ;
- tests d’ouverture et de fermeture ;
- prises de photos et vidéos du prototype.

Mon rôle principal est centré sur la partie physique et visuelle du projet afin de rendre le système fonctionnel et représentatif d’un véritable péage automatisé.

---

# Séance 1 — Présentation du projet

## Travail réalisé
Cette première séance a été consacrée à la découverte du module ainsi qu’à la présentation des objectifs et contraintes générales du projet.

Les principales contraintes identifiées sont :
- réalisation d’un système communicant ;
- utilisation de capteurs et d’actionneurs ;
- intégration d’une communication radio-fréquence ;
- suivi du projet sur GitHub.

Le groupe a ensuite choisi le projet **Automatic Gate System**, inspiré d’un système existant de contrôle d’accès automatique.

Le fonctionnement retenu repose sur :
- la détection d’un véhicule ;
- l’ouverture automatique d’une barrière ;
- une communication LoRa entre deux cartes UCA ;
- un futur système de gestion des places de parking.

## Mon implication
Lors de cette séance, j’ai participé :
- à la réflexion sur l’organisation globale du projet ;
- au choix de la maquette du parking ;
- à l’idée de représentation physique du péage ;
- aux premières discussions concernant la structure de la barrière automatique.

Cette séance a permis de définir mon rôle principal dans la partie mécanique et visuelle du projet.

---

# Séance 2 — Organisation du projet et choix du matériel

## Travail réalisé
Cette séance a permis d’organiser le projet de manière plus précise.

Le matériel utilisé a été identifié :
- carte UCA ;
- capteur ultrason HC-SR04 ;
- servomoteur ;
- modules LoRa 868 MHz ;
- LEDs ;
- écran OLED (prévu).

Le dépôt GitHub a également été mis en place afin de suivre l’évolution du projet et répartir le travail entre les différents membres du groupe.

## Mon implication
Pendant cette séance, j’ai participé :
- à la préparation de la future maquette ;
- à la réflexion sur l’emplacement de la barrière ;
- à l’organisation du parking à 4 places ;
- à la préparation du montage physique du servomoteur.

J’ai également commencé à réfléchir aux dimensions de la maquette et à la disposition des différents éléments afin de rendre le prototype clair et fonctionnel.

---

# Séance 3 — Validation de la détection et commande du servomoteur

## Objectif
Mettre en fonctionnement un premier prototype capable :
- de détecter une voiture ;
- d’ouvrir automatiquement une barrière.

---

## Câblage réalisé

### Servomoteur
- fil marron → GND
- fil rouge → V_bus
- fil jaune → D5

### Capteur ultrason HC-SR04
- GND → GND
- VCC → V_bus
- TRIG → A3
- ECHO → A2

---

## Fonctionnement obtenu

Le fonctionnement validé pendant cette séance est le suivant :

1. Le capteur ultrason mesure la distance.
2. Si une voiture est détectée à moins de 10 cm :
   - la présence est validée ;
   - le servomoteur effectue une rotation de 0° à 90° ;
   - la barrière s’ouvre.
3. Après un délai de quelques secondes :
   - le servomoteur revient à 0° ;
   - la barrière se referme.

Le moniteur série a permis de vérifier :
- les distances mesurées ;
- le bon fonctionnement du capteur ;
- la cohérence des seuils de détection.

---

## Principe de calcul de distance

Le capteur ultrason fonctionne grâce à la vitesse du son :

:contentReference[oaicite:0]{index=0}

Avec :
- \(d\) : distance ;
- \(v\) : vitesse du son (~343 m/s) ;
- \(t\) : temps aller-retour de l’onde ultrasonore.

---

## Mon implication

Pendant cette séance, mon travail a principalement porté sur :
- le montage physique de la barrière ;
- l’intégration mécanique du servomoteur ;
- les tests d’ouverture et fermeture ;
- les ajustements physiques du système ;
- les essais de rotation de la barrière ;
- la vérification du comportement réel de la maquette.

J’ai également participé aux essais pratiques afin de vérifier que la barrière s’ouvrait correctement lors de la détection d’un objet.

---

# Séance 4 — Communication LoRa et maquette du parking

## Objectif

Mettre en place une communication LoRa entre deux cartes UCA :

- une carte émettrice ;
- une carte réceptrice.

---

# Architecture du système

## Carte émettrice

### Rôle
- détecter une voiture avec le capteur ultrason ;
- envoyer un message LoRa lorsqu’une présence est détectée.

---

## Carte réceptrice

### Rôle
- recevoir le message LoRa ;
- commander l’ouverture de la barrière grâce au servomoteur.

---

# Communication LoRa

## Paramètres utilisés

- fréquence : **868 MHz**
- communication radio **LoRa point-à-point**
- transmission d’un message texte

### Exemple de message envoyé

```cpp
"Véhicule détecté"
```

---

# Fonctionnement obtenu

Le fonctionnement validé est le suivant :

1. Le capteur ultrason détecte une voiture.
2. La carte émettrice envoie un message LoRa.
3. La carte réceptrice reçoit le message.
4. Le servomoteur ouvre automatiquement la barrière.

---

# Mise en place de la maquette

Pendant cette séance, une maquette représentant un parking à 4 places a été ajoutée :

- P1
- P2
- P3
- P4

## Principe retenu

- lorsqu’une voiture entre :
  - le nombre de places disponibles diminue ;

- lorsque le parking atteint 4 véhicules :
  - affichage du message :

```cpp
"COMPLET"
```

---

# Affichage OLED

Un affichage OLED est prévu afin d’afficher :

- le nombre de places restantes ;
- l’état du parking ;
- les messages système.

---

## Branchement OLED

| Composant OLED | Carte UCA |
|---|---|
| VCC | Vcc_mcu (3.3V) |
| GND | GND |
| SCL | A5 |
| SDA | A4 |

---

## Exemple d’affichage OLED

```cpp
Places restantes : 2
Barriere ouverte
Vehicule detecte
```

---

# Mon implication

Pendant cette séance, mon travail a porté sur :

- l’intégration physique de la nouvelle maquette ;
- l’organisation des 4 places du parking ;
- les ajustements de la barrière sur la maquette ;
- les tests physiques après réception du message LoRa ;
- l’observation du comportement réel du système ;
- les photos et vidéos du prototype.

---

# Vérifications réalisées

J’ai également participé aux essais globaux afin de vérifier :

- la cohérence entre détection, transmission et ouverture ;
- la stabilité mécanique de la barrière ;
- le réalisme du prototype.

---

# Conclusion provisoire

À ce stade du projet :

- la détection par ultrason fonctionne ;
- la commande du servomoteur est validée ;
- la communication LoRa entre les deux cartes fonctionne ;
- la maquette du parking est opérationnelle.

---

# Améliorations prévues

Les prochaines étapes porteront sur :

- l’affichage OLED ;
- la gestion complète des 4 places ;
- l’amélioration de l’interface utilisateur ;
- l’optimisation du système.

