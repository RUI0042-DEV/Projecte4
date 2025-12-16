# Guia de Implementació: Servidor d'Impressió CUPS (PoC)

## 1. Introducció i Objectius

Aquesta guia detalla els passos per implementar una Prova de Concepte (PoC) per a la consultora EverPia. L'objectiu és configurar un servidor d'impressió centralitzat utilitzant CUPS sobre Ubuntu Server i validar el seu funcionament amb un client Zorin OS, utilitzant una impressora virtual PDF per simular la impressió en xarxa sense gastar paper.

Escenari de Treball:

Servidor: Ubuntu Server (Interfícies: NAT + Host-Only).

Client: Zorin OS Desktop (Interfícies: NAT + Host-Only).

Xarxa: Ambdues màquines han de tenir connectivitat entre elles (Xarxa NAT/Host-Only).

## 2. Instal·lació de Paquets (Servidor)

Primer, actualitzem els repositoris i instal·lem el servei CUPS i la impressora virtual PDF al servidor Ubuntu.

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install cups
sudo apt install cups-pdf
```

Això instal·la el sistema d'impressió (``cups``) i el paquet que genera PDFs (``cups-pdf``) en lloc d'imprimir físicament.

## 3. Configuració del Servei CUPS (Servidor)

Per permetre l'accés des de la xarxa i l'administració remota, cal editar l'arxiu de configuració principal.

### 3.1 Còpia de seguretat i edició

Abans de tocar res, fem una còpia de seguretat:

```bash
sudo cp /etc/cups/cupsd.conf /etc/cups/cupsd.conf.bak
sudo nano /etc/cups/cupsd.conf
```

<img width="659" height="60" alt="image" src="https://github.com/user-attachments/assets/5bfab956-be01-48e9-ace9-fa57903ab491" />

### 3.2 Modificacions a /etc/cups/cupsd.conf

Cal cercar i modificar les següents línies per permetre connexions externes (no només localhost):

Port d'escolta: Canvia ``Listen localhost:631`` per:
```
Port 631
```

> Això permet que CUPS escolti peticions de qualsevol interfície de xarxa.

Visibilitat a la xarxa: Assegura't que aquestes opcions estan activades:

```
Browsing On
BrowseLocalProtocols dnssd
```

<img width="1103" height="584" alt="Captura de pantalla 2025-12-16 160615" src="https://github.com/user-attachments/assets/06bb7ef7-a809-4582-b0bf-f640cae95454" />

Permisos d'accés (Locations): Afegeix ``Allow @LOCAL`` a les seccions ``<Location />`` i ``<Location /admin>`` per permetre l'accés des de la xarxa local.

```
<Location />
  Order allow,deny
  Allow @LOCAL
</Location>

<Location /admin>
  AuthType Default
  Require valid-user
  Order allow,deny
  Allow @LOCAL
</Location>
```

<img width="1101" height="579" alt="image" src="https://github.com/user-attachments/assets/9229e591-fba4-45cc-8844-3d68ddaa3fb5" />

---

## 4. Gestió d'Usuaris i Reinici del Servei (Servidor)

### 4.1 Afegir l'usuari al grup d'administració

Per poder afegir impressores via web sense rebre un error de "Prohibit", cal afegir el teu usuari d'Ubuntu al grup ``lpadmin``.

```bash
sudo usermod -aG lpadmin el_teu_usuari
```

<img width="471" height="43" alt="image" src="https://github.com/user-attachments/assets/607c7fe7-4e11-49a4-8ffd-d4c74ebeafab" />

> (Substitueix el_teu_usuari pel nom real del teu usuari, ex: usuari).

### 4.2 Reiniciar CUPS

Aplica els canvis reiniciant el servei:

```bash
sudo systemctl restart cups
sudo systemctl status cups
```

> Comprova que l'estat és active (running).

<img width="883" height="368" alt="image" src="https://github.com/user-attachments/assets/5b1378a6-19f2-480a-bd31-6aae06b60895" />

## 5. Afegir la Impressora via Web (Client/Servidor)

- Ara configurarem la impressora utilitzant la interfície web. Pots fer-ho des del navegador del client Zorin o des del mateix servidor (si tingués entorn gràfic, o via forwarding). Assumirem accés des del client.

- Obre el navegador i vés a: https://IP_DEL_SERVIDOR:631 (Accepta l'avís de seguretat SSL).

- Navega a la pestanya Administration (Administració).

- Fes clic a Add Printer (Afegir Impressora). Et demanarà usuari i contrasenya (usa l'usuari que has posat al grup lpadmin al pas 4.1).

- Selecció: Tria CUPS-PDF (Virtual PDF Printer) a "Local Printers".

- Detalls:
  - Name: Virtual_PDF_Printer
  - Description: Virtual PDF Printer
  - Location: Server
  - Sharing: Marca la casella Share this printer (Compartir aquesta impressora).

- Marca/Model: Selecciona Generic -> Generic CUPS-PDF Printer (no options) i finalitza.

## 6. Configuració del Client (Zorin OS)
