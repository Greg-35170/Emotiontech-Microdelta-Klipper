# Emotiontech-Microdelta-Klipper
Configuration Klipper pour Imprimante Emotiotech Microdelta Rework

## Configuration du Raspberry
(A minima un Raspberry Pi 3)

En utilisant [Pi Imager](https://www.raspberrypi.com/software/)
* Choisir le modèle de Raspberry
* Choisir l'OS
  * "Other Specific-purpose OS"
  * 3D Printing
  * Mainsail OS (64 bits)
* Modifier les réglages
  * Compléter l'onglet "Général"
  * Activer SSH dans l'onglet "Services"
* Lancer en activant les modifications

Mettre la carte dans le Raspberry et démarrer

## Génération du firmware
* Se connecter en SSH via un autre pc
```bash
ssh votreid@192.168.XXX.XXX # Selon votre id et adresse ip du raspberry
```
* Saisir mot de passe
* Une fois connecté:
  * Commande:
```bash
cd klipper
make menuconfig
```
  Choisir les options suivantes:
  * Micro-controller Architecture (LPC176x)
  * processor model (lpc1768 (10 MHz))
  * Bootloader offset (16KiB bootloader)
  * Communication interface (USB)

  Quitter et sauvegarder
* Retour dans la console
* Commande:
```bash
make clean
make
```
La commande make créé un fichier klipper.bin dans le répertoire /out qu'il faut récupérer
* Commande depuis le pc dans une nouvelle console (pas en SSH)
```bash
scp votreid@192.168.XXX.XXX:~/klipper/out/klipper.bin c:/votre/chemin/
```

## Flashage de l'imprmante
* Renommer ce fichier en firmware.bin
* copier sur la carte micro SD de l'imprimante
* Démarer l'imprimante
* Patienter un peu
* Connecter l'imprimante au Raspberry via un cable usb

Dans la console en SSH, récupérer le serial de l'imprimante:
* taper la commande
```bash
ls /dev/serial/by-id/*
```
* Enregistrer dans un fichier texte l'adresse retournée par la console du type: /dev/serial/by-id/usb-Klipper_lpc1768_1DD00008C994FAAEA2D6DD57C42000F5-if00
* Fermer la console SSH

## Configuration Mainsail
Depuis un navigateur saisir l'adresse ip du rasberry pi. Vous arrivez normalement sur l'interface Mainsail.

Dans l'onglet "Machine" importer dans le répertoire config les fichiers de ce dépot:
  * printer.cfg
  * macro.cfg

* Dans Mainsail, ouvrir le fichier printer.cfg
* Remplacer le serial mcu par celui récupéré précédemment
* Save and restart

Vous devriez maintement être connecté avec l'imprimante (tentez un home en gardant le doigt sur le bouton ON/OFF.... au cas ou...)

## Configuration slicer
* Gcode de démarrage:
```plaintext
START_PRINT EXTRUDER_TEMP=[nozzle_temperature_initial_layer] BED_TEMP=[bed_temperature_initial_layer_single]
```
* Gcode de fin:
```plaintext
END_PRINT
```

## Calibration
Dans Mainsail, dans l'onglet "Tableau de bord"
Executer les macros: Attention il faut mettre en place le palpeur pour toute ces macros
* Z_OFFSET_CALIBRATION 
* DELTA_CALIBRATION 
* BED_LEVELING

L'imprimante est prête pour sa première impression!

