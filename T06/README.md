# 🖥️ T06: PoC Suport Gràfic amb Escriptori Remot (RDP)

## 🎯 Objectiu
Aquesta tasca és la **Prova de Concepte (PoC)** per documentar l'estratègia de la consultora per a l'accés remot amb interfície gràfica, fonamental per al **suport directe a l'usuari final** i l'administració de servidors amb GUI.

L'objectiu és crear les **Guies de Suport Gràfic Oficials** que utilitzaran els tècnics per establir connexions d'escriptori remot de manera ràpida i eficaç, centrant-nos en el protocol **RDP**.

---

## 🚀 La Vostra Missió: Documentació RDP Multiplataforma

El protocol **RDP (Remote Desktop Protocol)** no només és l'estàndard de facto per a Windows, sinó que gràcies a la incorporació de suport a entorns com **GNOME (utilitzat per Zorin OS)**, ens permet oferir una experiència de suport unificada.

Heu de crear una guia que abordi els dos escenaris principals d'accés remot gràfic que trobarem als equips dels nostres clients i servidors.

### 1. 🪟 Escenari RDP: Client a Windows 11
Documenteu la configuració i connexió de l'Escriptori Remot per als clients Windows, que són molt habituals:

* **Configuració de l'Host (Destí):** Passos per habilitar el servei d'Escriptori Remot en un equip Windows 11.
* **Connexió des del Client:** Ús de l'aplicació nativa **"Connexió a Escriptori Remot"** (mstsc.exe) per connectar-se a l'equip Windows remot.
* **Consideracions de Seguretat:** Menció a la importància de credencials segures.

### 2. 🐧 Escenari RDP: Client a Zorin OS / GNOME
Documenteu com connectar-se als equips Linux que utilitzen els nostres clients (Zorin OS) aprofitant el suport natiu de GNOME per a RDP:

* **Configuració de l'Host (Destí):** Passos per habilitar l'Escriptori Remot basat en RDP a la configuració de Zorin OS / GNOME.
* **Connexió des del Client:** Ús d'un client RDP compatible des de l'estació de treball (pot ser un client Windows o un client Linux com **Remmina**).
* **Demostració:** Com es veu la sessió remota gràfica en un entorn Linux.

### 3. 🚨 Escenari de Suport Directe
A la guia, destaqueu la importància de la **velocitat** de la connexió (RTO) en un escenari de suport a l'usuari final nerviós.

* **Procediment Ràpid:** Definició del flux de treball per connectar-se en menys de 5 minuts quan un client truca amb una incidència.

---

## 📝 Documentació i Lliurament

El lliurament ha de ser una guia tècnica, amb captures de pantalla clares, que abordi la configuració i l'ús del protocol RDP en ambdós entorns (Windows i Zorin/GNOME).

### Punts Clau a Incloure:

* Configuració dels hosts (Windows 11 i Zorin OS) per acceptar connexions RDP.
* Com utilitzar l'eina client per a la connexió.
* Gestió de les credencials d'usuari.

---

## 🔗 Materials de Suport

* Moodle 0227 Serveis de Xarxa. UD4.AA3 Escriptoris Remots
---
[Tornar a la pagina principal](../README.md)
