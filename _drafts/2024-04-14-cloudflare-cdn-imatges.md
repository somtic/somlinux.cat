---
title: "Fer servir Cloudflare com a CDN per web"
description: >-
  Registre i configuració de Cloudflare per fer servir un CDN per les imatges
  d'aquesta web
author: jordan
date: 2024-04-14 20:55:00 +0200
categories: [Web, Tutorial]
tags: [Web]
pin: false
img_path: '/posts/20180809'

---

# Introducció



## Cloudflare compte gratuit

1. Registrar-se
2. Afegir un domini
3. Proxy dns (automàtic després d'afegir un domini
4. Cloudflare assigna uns servidors DNS per canviar al registrador del domini

kimora.ns.cloudflare.com

luke.ns.cloudflare.com

4.1 accedir al registrador del domini per canviar dns
4.2 Desactivar DNSSEC
4.3 Canviar nameserver del domini
4.4 Quan ok (24 hores) tornar a activar DNSSEC

Comprovar propagació DNS

https://www.dnswatch.info/
https://dnschecker.org/


Configurar rDNS (Reverse DNS)

dig -x 38.242.223.229 +noall +answer

229.223.242.38.in-addr.arpa. 43200 IN	PTR	vmi804892.contaboserver.net.

