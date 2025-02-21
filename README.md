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
* Renommer ce fichier en firmware.bin
* copier sur la carte micro SD de l'imprimante
* Démarer l'imprimante
* Patienter un peu

Depuis un navigateur saisir l'adresse ip du rasberry pi
Vous arrivez normalement sur mainsail



