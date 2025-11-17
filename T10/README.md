# 🖨️ T10: Servidor d'Impressió Linux: Implementació CUPS (PoC)

## 🎯 Objectiu
Implementar un **Servidor d'Impressió Centralitzat** utilitzant **CUPS (Common Unix Printing System)** en un servidor Linux. L'objectiu és demostrar al client **DevOptimize Solutions** una solució d'impressió eficient i centralitzada per als seus entorns Linux (Ubuntu Server i Zorin OS Clients).

Aquesta tasca és una **Prova de Concepte (PoC)** individual que simula l'ús d'una impressora de xarxa mitjançant la impressora virtual `cups-pdf`.

---

## 💼 Cas Client: DevOptimize Solutions
El client busca estandarditzar i simplificar la gestió d'impressió en el seu entorn Linux.

### Escenari de Treball (Continuació de T09):
* **Màquina 1 (Servidor):** Ubuntu Server (2 interfícies de xarxa: NAT + Host-Only).
* **Màquina 2 (Client):** Zorin OS (Desktop) (Mateixa configuració de xarxa).

---

## 🛠️ Prova de Concepte (PoC): Implementació de CUPS-PDF

Heu de configurar l'entorn per fer que el servidor actuï com a gestor d'impressió i que el client pugui enviar feines.

### Passos de Configuració i Validació:

1.  **Instal·lació de CUPS:** Instal·lar el paquet principal de CUPS al servidor Ubuntu.
2.  **Instal·lació de l'Impressora Virtual:** Instal·lar el paquet `cups-pdf` per simular la impressora física.
3.  **Configuració de l'Administració de CUPS:**
    * Configurar l'administració de CUPS per permetre l'accés remot al frontal web.
    * Modificar la configuració (normalment a `/etc/cups/cupsd.conf`) perquè CUPS escolti per **totes les interfícies de xarxa** i permeti l'accés des de la xarxa Host-Only.
    * Afegir un usuari amb permisos d'administració a CUPS.
4.  **Compartició de la Impressora (Web CUPS):**
    * Accedir al frontal web de CUPS des del navegador del servidor o del client (utilitzant l'adreça IP del servidor i el port 631).
    * Utilitzar la interfície web per **compartir** la impressora virtual (`cups-pdf`), assegurant-se que sigui visible per la xarxa.
5.  **Configuració del Client Zorin:**
    * Al sistema operatiu Zorin OS (Client), afegir la impressora de xarxa compartida (ja sigui automàticament o manualment mitjançant l'adreça IP del servidor).
6.  **Prova d'Impressió:**
    * Des del client Zorin, enviar diversos documents de prova a la impressora compartida (CUPS-PDF).
7.  **Validació al Servidor:**
    * Comprovar al servidor la carpeta on es guarden els fitxers PDF generats (normalment a `~/PDF` o una ruta similar, depenent de la configuració de `cups-pdf`).
    * Verificar que hi ha tants fitxers PDF com treballs d'impressió s'han enviat des del client.

---

## 📝 Documentació i Lliurament

El lliurament ha de ser una guia tècnica que documenti tot el procés.

### Punts Clau a Incloure:

* Comandes d'instal·lació de CUPS i `cups-pdf`.
* Modificacions realitzades al fitxer de configuració de CUPS (`cupsd.conf`) per permetre l'accés remot (escoltar totes les interfícies).
* Captura de pantalla de la interfície web de CUPS que mostri la impressora compartida.
* Captura de pantalla del client Zorin amb la impressora afegida.
* Prova final al servidor que mostri els fitxers PDF generats a la carpeta de destí, confirmant el correcte flux de la feina d'impressió.

---

## 🔗 Materials de Suport

* Material propi: UD5. AA1. CUPS.
* J.B. Alex Mantich. (2024). [Instalación de servidor de impresión en cups para linux (Vídeo).](https://www.youtube.com/watch?v=FNwSTrOSgZQ)
* Idroot. (2025). [How To Install CUPS Print Server on Ubuntu 24.04 LTS.](https://idroot.us/install-cups-print-server-ubuntu-24-04/)
---
[Tornar a la pagina principal](../README.md)
