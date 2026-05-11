# Contribution personnelle TSHERGIZBIEV Aydima - Partie logique du système 

## Séance 1 - Présentation du projet 

Cette première séance a été consacrée à la présentation du module, des objectifs du projet ainsi que des contraintes générales à respecter.

Les consignes principales ont été identifiées :
- réalisation d'un systèmes communicant ;
- utilisation de capteurs et d'actionneurs ;
- intégration d'une communication radio ;
- mise en place d'un suivi du projet sur GitHub.

Cette séance a permis de comprendre le cadre global du projet et d'identifier les compétences technique qui devront être mobilisées par la suite.

---

## Séance 2 - Définition du projet et répartition des rôles

Cette séance a permis de préciser l'orientation du projet et d'organiser le travail au sein du groupe.

Le choix retenu est celui du projet **Automatic Gate System**. Le groupe s'est appuyé sur un projet existant utilisant une détection RFID, mais le fonctionnement a été adapté en remplaçant ce dispositif par un **capteur ultrason**, plus cohérent avec le matériel disponible et avec le niveau attendu dans le cadre du module.

Une première répartition des tâches a également été définie. Ma responsabilité porte sur :
- la détection de présence par un capteur ultrason ;
- la logique du focntionnement du système ;
- les conditions d'ouverture et de fermeture de la barrière.

Cette séance a permis de définir plus précisément mon rôle dans le projet, centré sur la partie logique et décisionnelle du système.

---

## Séance 3 - Validation du principe de détection et commande du servomoteur

Cette séance a été consacrée à la mise en fonctionnement d'un premier prototype à partir du code fourni.

Le travail réalisé a porté sur l'interaction entre : 
- le **capteur ultrason**, chargé de mesurer la distance ;
- le **servomoteur**, chargé de simuler l'ouverture et la fermeture de la barrière.

## Câblage réalisé

Le montage utilisé pendant cette séance reposait sur les branchements suivants :

### Servomoteur 
- fil **marron** relié à **GND**
- fil **jaune** relié à **D5**
- fil **rouge** relié à **V_bus**

### Capteur ultrason 
- **GND** relié à **GND**
- **ECHO** relié à **A2**
- **TRIG** relié **A3**
- **VCC** relié à **V_bus**

Ces branchements ont permis de relier correctement les deux composants à la carte afin de tester la détection de présence et la commande du servomoteur.

## Fonctionnement observé

Le fonctionnement observé est le suivant :
- lorsque la distance mesurée devient **inférieure à 10 cm**, une présence est détectée ;
- le servomoteur effectue alors une rotation de **0 à 90 degrés**, correspant à l'ouverture de la barrière ;
- après un délai défini dans le programme, le servomoteur revient de **90 à 0 degré**, correspondant à la fermeture.

L'analyse du **moniteur série** a également permis de confirmer le comportement du capteur :
- en présence d'un objet proche, les valeurs mesurées sont faibles ;
- en l'absence d'objet proche, les valeurs remontent à des valeurs plus élvées.

## Ma contribution dans cette séance

Dans le cadre de ma partie, cette séance a principalement permis de valider la logique du système à partir de la condition :

```cpp
// Si la distance mesurée par le capteur ultrason est inférieure à 10 cm,
// on considère qu'un véhicule est présent devant la barrière.
if (distance < 10.00) {
    Serial.println("Présence détectée");

    // Ouverture de la barrière : le servomoteur passe de 0° à 90°
    for (pos = 0; pos <= 90; pos += 1) {
        myservo.write(pos);
        delay(6);
    }

    // La barrière reste ouverte pendant 5 secondes
    delay(5000);

    // Fermeture de la barrière : le servomoteur revient de 90° à 0°
    for (pos = 90; pos >= 0; pos -= 1) {
        myservo.write(pos);
        delay(6);
    }
}
else {
    // Si aucun véhicule n'est détecté à moins de 10 cm,
    // la barrière reste fermée.
    Serial.println("Aucune présence détectée");
} 
```

Cette condition représente la logique principale du système.  
Le capteur ultrason mesure la distance en continu. Lorsque cette distance devient inférieure à 10 cm, le programme considère qu’un véhicule est présent devant la barrière. Le servomoteur est alors commandé pour ouvrir la barrière, attendre quelques secondes, puis la refermer automatiquement.

---         

## Séance 4 – Mise en place de la logique de détection et communication LoRa

## Présentation de la séance

Cette séance a été consacrée au développement de la logique de fonctionnement du système de barrière intelligente.

L’objectif n’était plus seulement de tester les composants séparément, mais de construire une logique décisionnelle complète permettant au système de réagir automatiquement lorsqu’un véhicule est détecté.


## Mise en place de la logique de détection

Le fonctionnement du système repose sur l’utilisation d’un capteur ultrason HC-SR04.  
Ce capteur permet de mesurer la distance entre le capteur et un objet placé devant la barrière.

La première étape a consisté à définir une condition permettant d’identifier la présence d’un véhicule.

```cpp
if (distance < 10.00)
```

Cette condition signifie que lorsque la distance mesurée devient inférieure à **10 cm**, le système considère qu’un véhicule est présent devant la barrière.

Ce seuil permet de transformer une simple mesure physique en une information logique exploitable par le programme.


## Mesure de la distance

La distance est calculée à partir du temps de retour du signal ultrason.

```cpp
digitalWrite(trigPin, LOW);
delayMicroseconds(2);

digitalWrite(trigPin, HIGH);
delayMicroseconds(10);

digitalWrite(trigPin, LOW);

duration = pulseIn(echoPin, HIGH);
distance = (duration * 0.0343) / 2;
```

Le capteur envoie une impulsion ultrason, puis mesure le temps nécessaire pour que le signal revienne après avoir rencontré un obstacle.

La formule utilisée est :

```text
distance = (durée × vitesse du son) / 2
```

La division par 2 est nécessaire car le signal effectue un aller-retour entre le capteur et l’objet.


## Analyse et traitement de l’information

Une fois la distance mesurée, le système analyse la valeur obtenue.

La logique mise en place repose sur le principe suivant :

```text
Si un véhicule est détecté :
    le système valide la présence
    une information est transmise
    une action peut être déclenchée

Sinon :
    aucune action n’est exécutée
```

L’objectif est donc de mettre en œuvre une chaîne logique complète :

```text
Mesure
→ Analyse
→ Validation
→ Transmission
→ Action
```

Cette organisation permet d’obtenir un comportement automatisé cohérent.


## Communication logique entre les deux cartes

Durant cette séance, le système a été modifié afin de fonctionner avec deux cartes UCA distinctes communiquant via LoRa.

La première carte joue le rôle d’émetteur. Elle est chargée de :

- mesurer la distance ;
- analyser la condition de détection ;
- décider si un véhicule est présent ;
- envoyer une information à la seconde carte.

Le message envoyé correspond à :

```cpp
payload = "Véhicule détecté";
```

Dans la logique retenue, le message `"Vehicule détecté"` signifie qu’un véhicule a été détecté.

La seconde carte reçoit ensuite ce message et peut déclencher l’ouverture de la barrière.

Cette organisation permet de séparer :

- la partie **détection et décision** ;
- la partie **réception et action mécanique**.

---

## Gestion des détections multiples

Un problème important a été identifié durant les tests.

Lorsque le véhicule reste immobile devant le capteur, le programme continue de passer dans la boucle `loop()`.  
Sans correction, le même véhicule peut donc être détecté plusieurs fois, ce qui provoquerait plusieurs envois LoRa pour une seule voiture.

Pour éviter ce problème, une variable booléenne a été ajoutée :

```cpp
// Anti double détection
bool voitureDetectee = false;
```

Cette variable permet de mémoriser l’état de détection du véhicule.

La logique est la suivante :

```text
Première détection :
    détection autorisée
    message envoyé

Tant que le véhicule reste devant le capteur :
    nouvelles détections bloquées

Lorsque le véhicule s’éloigne :
    réinitialisation du système
    nouvelle détection possible
```

Dans le code, cela donne :

```cpp
if (distance < 10.00 ) {

    payload = "Véhicule détecté";

    LoRa.beginPacket();
    LoRa.print(payload);
    LoRa.endPacket();
}
```


## Réarmement du système

Pour pouvoir détecter un nouveau véhicule, il faut réinitialiser la variable `voitureDetectee`.

Le réarmement est effectué lorsque la distance redevient supérieure à un second seuil :

```cpp
if (distance > 15.00) {
    voitureDetectee = false;
}
```

Ce seuil de **15 cm** permet de considérer que le véhicule s’est éloigné du capteur.

L’utilisation de deux seuils différents permet d’éviter des déclenchements trop rapides autour de la limite des 10 cm.


## Structure logique globale du système

À ce stade du projet, la logique de fonctionnement peut être représentée ainsi :

```text
Véhicule devant le capteur
↓
Mesure de la distance
↓
Comparaison avec le seuil de 10 cm
↓
Vérification de l’état de détection
↓
Validation de la présence
↓
Envoi du message LoRa "Véhicule détecté"
↓
Réception par la seconde carte
↓
Déclenchement de l’ouverture de la barrière
```

Cette structure permet d’obtenir un système plus intelligent qu’une simple barrière locale, car l’action est déclenchée à distance après transmission de l’information.

---

## Logique finale simplifiée

La logique principale peut être résumée avec le pseudo-code suivant :

```text
Mesurer la distance

Si distance < 10 cm :
    considérer qu’un véhicule est présent
    bloquer les nouvelles détections
    envoyer le message "Véhicule détecté" via LoRa

Si distance > 15 cm :
    réinitialiser la détection
    autoriser une nouvelle détection
```


## Tests réalisés

Plusieurs tests ont été réalisés afin de vérifier le bon fonctionnement de la logique mise en place :

- vérification des valeurs de distance dans le moniteur série ;
- validation du seuil de détection à 10 cm ;
- contrôle de l’envoi du message `"Véhicule détecté"` ;
- vérification de la réception du message sur la seconde carte ;
- validation du blocage des détections multiples ;
- vérification du réarmement du système lorsque le véhicule s’éloigne.

Ces tests ont permis de confirmer que la logique de décision fonctionne correctement.


## Résultat obtenu

À l’issue de cette séance, le système est capable de :

- mesurer une distance avec le capteur ultrason ;
- détecter automatiquement la présence d’un véhicule ;
- analyser une condition logique ;
- éviter les détections répétées d’un même véhicule ;
- transmettre une information via LoRa ;
- déclencher une action à distance sur une seconde carte.

Cette séance permet de rattacher la logique de fonctionnement du système, la prise de décision automatique et le traitement des informations échangées entre les différents modules du prototype.

























