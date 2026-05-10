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
