---
title: "Còpia de seguretat d'arxius i base de dades"
description: "Com fer una còpia de seguretat d'arxius i base de dades en un servidor vps debian linux"
date: 2024-04-12 20:00:39 +0200
categories: [Linux, bash]
tags: [backups, vps, mysql, tar, hosting, manuals]
image:
  path: "https://somlinux.cat/assets/img/posts/lukas-NLSXFjl_nhc-unsplash-debian.jpg"
  alt: "Foto de Lukas a Unsplash"
---

# Introducció

Soc dels que ha ficat moltes vegades la pota per actualitzar alguna cosa sense haver fet abans una còpia de seguretat.

Quan utilitzava allotjament en servidors compartits per a les meves webs, trobava molt tediós descarregar tots els arxius per FTP i, després, fer una còpia de seguretat de la base de dades a través de phpMyAdmin.

Ara que administro el servidor VPS, fer aquestes còpies de seguretat a través de SSH és molt més ràpid, ja que només necessito dues comandes.

1. Comanda per copia i empaquetar els arxius.
2. Comanda par fer un backup de la base de dades.

## Còpia i empaquetat .tar.gz del directori

Depenent de quin panell de control de hosting web feu servir (hestiacp, ispconfig) tindreu una organització de directoris o altres on s'emmagatzemen les webs.

L'exemple està realitzat amb ispconfig.

- L'aplicació web es troba a `/var/www/[elteudomini]/web/`
- I el directori per backups a `/var/www/[elteudomini]/backup/`

Em situo dins de /backup/ i faig servir una comanda per copiar i empaquetar.

```bash
tar -czvf elteudomini_backup.tar.gz /var/www/[elteudomini]/web/
```

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

```bash
mysqldump -u usuari_base_dades -p'paraulaclau' base_dades_web | gzip > backup_base_dades.sql.gz
```

Aquestes comandes estan realitzades amb l'usuari root. La recomanació és fer-ho amb l'usuari que pertany a aquell hosting, posant davant de les instruccions `sudo -u usuari`

## Restaurar base de dades Mysql

Tenim l'arxiu comprimit (.gz) amb la base de dades. El primer que hem de fer és descomprimir:

```bash
gunzip base_dades_web.sql.gz
```

Ara ja tenim l'arxiu .sql i l'hem d'importar a la base de dades que tinguem (si no la teniu, l'heu de crear primer). Farem servir la comanda `mysql`

```bash
mysql -u usuari_base_dades -p'paraulaclau' base_dades_web < arxiu.sql
```




