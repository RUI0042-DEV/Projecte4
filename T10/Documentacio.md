# Guia de Implementació: Servidor d'Impressió CUPS (PoC)

## 1. Introducció i Objectius

Aquesta guia detalla els passos per implementar una Prova de Concepte (PoC) per a la consultora EverPia. L'objectiu és configurar un servidor d'impressió centralitzat utilitzant CUPS sobre Ubuntu Server i validar el seu funcionament amb un client Zorin OS, utilitzant una impressora virtual PDF per simular la impressió en xarxa sense gastar paper.

Escenari de Treball:

Servidor: Ubuntu Server (Interfícies: NAT + Host-Only).

Client: Zorin OS Desktop (Interfícies: Xarxa NAT + Host-Only).

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

<img width="972" height="698" alt="image" src="https://github.com/user-attachments/assets/025cd06c-61f6-4476-816f-c0a99339ded3" />

- Navega a la pestanya Administration (Administració).

<img width="981" height="709" alt="image" src="https://github.com/user-attachments/assets/b247dc2e-a6ad-43c5-97ad-c60d817fad67" />
> Aquí hauràs de posar l'usuari del server.

- Fes clic a Add Printer (Afegir Impressora). Et demanarà usuari i contrasenya (usa l'usuari que has posat al grup lpadmin al pas 4.1).

- Selecció: Tria CUPS-PDF (Virtual PDF Printer) a "Local Printers".

<img width="969" height="671" alt="image" src="https://github.com/user-attachments/assets/223d1ee9-1541-4761-993c-ecefe1ced07e" />

- Detalls:
  - Name: Virtual_PDF_Printer
  - Description: Virtual PDF Printer
  - Location: Server
  - Sharing: Marca la casella Share this printer (Compartir aquesta impressora).

<img width="977" height="677" alt="image" src="https://github.com/user-attachments/assets/31861948-caef-4e1a-98fc-c98e8b9b7a04" />

- Marca/Model: Selecciona Generic -> Generic CUPS-PDF Printer (no options) i finalitza.

<img width="978" height="705" alt="image" src="https://github.com/user-attachments/assets/df86b4d7-eeb6-4664-a13e-1d55614efedc" />

## 6. Configuració del Client (Zorin OS)

Al client (Zorin OS / Ubuntu Desktop):

- Vés a Configuració -> Impressores.
- Si estan a la mateixa xarxa, la impressora Virtual_PDF_Printer_server hauria d'aparèixer automàticament.
- Si no apareix, fes clic a "Afegir Impressora" i busca-la per la IP del servidor.

<img width="978" height="678" alt="image" src="https://github.com/user-attachments/assets/d985c683-7f7b-475c-988b-2303cebbc1ba" />

## 7. Prova de Funcionament i Verificació

### 7.1 Enviar una feina

Des del client Zorin, obre un document o navegador i imprimeix utilitzant la ``Virtual_PDF_Printer_server``.

<img width="976" height="644" alt="image" src="https://github.com/user-attachments/assets/dadd57f2-c966-4e7d-9094-755032aae4f9" />

<img width="976" height="642" alt="image" src="https://github.com/user-attachments/assets/66092c50-bc41-4977-8ada-1aab846337e6" />

## 7.2 Verificar al panell web

A la web de CUPS (``https://IP:631``), a la pestanya Jobs (Treballs), hauries de veure l'estat del treball com a "completed".

<img width="970" height="679" alt="image" src="https://github.com/user-attachments/assets/70862355-0ddd-43c9-86a2-7f081702075c" />

## 7.3 Verificar el fitxer PDF (Evidència Final)

Al servidor Ubuntu, els documents impresos es guarden automàticament com a arxius PDF. Per defecte, solen anar a una carpeta PDF dins del home de l'usuari o a /var/spool/cups-pdf/ANONYMOUS/ depenent de la configuració. Segons la teva captura, apareixen a /home/usuari/PDF.

Comprova-ho amb:

```bash
ls -l ~/PDF
# o si tens 'tree' instal·lat
tree ~/PDF
```

Hauries de veure els fitxers ``.pdf`` generats.
