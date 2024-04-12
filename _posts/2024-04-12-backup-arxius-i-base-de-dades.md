---
title:  "Còpia de seguretat d'arxius i base de dades"
description: "Com fer una còpia de seguretat d'arxius i base de dades en un servidor vps debian linux"
date:   2024-04-12 20:00:39 +0200
categories: [linux][bash]
tags: [backups][vps][mysql][tar]
---

# Introducció

Soc dels que ha ficat moltes vegades la pota per actualitzar alguna cosa sense haver fet abans una còpia de seguretat.

Quan feia servir allotjament per webs en servidors compartits, em feia molta mandra fer una descàrrega de tots els arxius per FTP, i desprès fer una còpia de seguretat de la base de dades a través de phpmyadmin.

Ara que m'administro jo mateix el servidor vps, fer aquestes còpies de seguretat a través de ssh és molt més ràpid, ja que sols he de fer servir dues comandes.

1. Comanda per copia i empaquetar els arxius.
2. Comanda par fer un backup de la base de dades.

## Còpia i empaquetat .tar.gz del directori

Depenent de quin panell de control de hosting web feu servir (hestiacp, ispconfig) tindreu una organització de directoris o altres on s'emmagatzemen les webs.

L'exemple està realitzat amb ispconfig.

- L'aplicació web es troba a `/var/www/[elteudomini]/web/`
- I el directori per backups a `/var/www/[elteudomini]/backup/`

Em situo dins de /backup/ i faig servir una comanda per copiar i empaquetar.

`tar -czvf elteudomini_backup.tar.gz /var/www/[elteudomini]/web/`

- -c: Crea un nou arxiu tar.
- -z: Comprimeix l\'arxiu amb gzip.
- -v: Mostra el progrés de la compressió de forma detallada (verbose).
- -f: Indica el nom de l\'arxiu tar que es crearà o modificarà.

## Còpia de la Base de dades (mysqldump)

Per la base de dades (mysql) tenim els següents paràmetres:

- base de dades: base_dades_web
- usuari base de dades: usuari_base_dades
- password usuari: paraulaclau

Situats al mateix directori farem la comanda:

`mysqldump -u usuari_base_dades -p'paraulaclau' base_dades_web | gzip > backup_base_dades.sql.gz`

Aquestes comandes estan realitzades amb l'usuari root. La recomanació és fer-ho amb l'usuari que pertany a aquell hosting, posant davant de les instruccions `sudo -u usuari`
