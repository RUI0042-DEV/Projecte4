# 📂 T09: Servidor de Fitxers Linux: Implementació NFSv3

## 🎯 Objectiu
Implementar i demostrar una solució de servidor de fitxers centralitzat utilitzant **NFS (Network File System)** per al client **DevOptimize Solutions**. L'objectiu és resoldre el problema de descontrol i errors de versió del codi font en el seu entorn exclusivament Linux.

Aquesta tasca és una **Prova de Concepte (PoC)** individual per demostrar el control d'accés a fitxers i les limitacions de NFS sense autenticació centralitzada (LDAP/NIS).

---

## 💼 Cas Client: DevOptimize Solutions
El client requereix la centralització del seu codi i actius digitals. Com que l'entorn és 100% Linux, s'ha proposat **NFSv3** com la solució nativa més eficient.

### Context Crític:
* **Problema:** Còpies locals del codi, provocant errors de versió constants.
* **Restricció:** El client no té (ni vol implementar de moment) un entorn d'autenticació centralitzada. La gestió d'usuaris i permisos es farà de forma **local i manual** (limitació a demostrar).

---

## 🛠️ Requisits de la Prova de Concepte (PoC)

Heu de configurar un entorn de proves amb un Servidor Linux i un Client Linux (màquines virtuals) i realitzar els següents passos de configuració i validació:

### 1. ⚙️ Configuració del Servidor NFS
1.  **Instal·lació:** Instal·lar els paquets necessaris de NFS (permetent NFSv3).
2.  **Creació de Recursos:** Crear una carpeta de prova (p. ex., `/srv/devoptimize/`) que allotjarà els fitxers de codi font.
3.  **Configuració d'Usuaris i Grups:**
    * Crear usuaris i grups locals (p. ex., `devs`, `tester`) per simular el personal de DevOptimize.
    * Assignar la propietat de la carpeta de recursos (`chown`, `chmod`) per establir el control d'accés inicial.
4.  **Exportació de la Carpeta (`/etc/exports`):**
    * Configurar l'exportació de la carpeta NFS, especificant les opcions d'accés (p. ex., `rw`, `sync`, `no_subtree_check`).
    * Limitar l'accés només a la xarxa o a adreces IP específiques dels clients.
5.  **Firewall:** Habilitar el trànsit NFS (ports) al firewall del servidor.
6.  **Reinici/Recàrrega:** Assegurar que les exportacions estiguin actives (`exportfs -a`).

### 2. 🖥️ Configuració del Client NFS
1.  **Instal·lació:** Instal·lar els paquets clients de NFS.
2.  **Muntatge Manual:** Realitzar un muntatge manual de la carpeta compartida des del servidor (p. ex., `/mnt/codi`).
3.  **Muntatge Persistents (`/etc/fstab`):** Configurar el muntatge persistent per assegurar que el recurs estigui disponible després de cada reinici.

### 3. ✅ Demostració i Control d'Accés

Una vegada configurat, s'ha de demostrar el control de permisos utilitzant el client:

1.  **Simulació d'Usuaris:** Assegurar que l'usuari local del client té un **UID/GID idèntic** al del servidor per simular l'entorn de DevOptimize.
2.  **Prova de Permisos:**
    * Intentar crear, modificar i esborrar fitxers des del client amb un usuari que té permisos d'escriptura.
    * Intentar crear fitxers des del client amb un usuari que **no** té permisos d'escriptura (només lectura) i demostrar el bloqueig.
3.  **Validació del Root Squash:** Demostrar que l'usuari `root` del client no té privilegis d'escriptura sobre la carpeta exportada (utilitzant l'opció `root_squash` o similar per defecte).

---

## 📝 Documentació i Lliurament

El lliurament ha d'incloure els següents punts, documentats de forma clara amb comandes i les corresponents captures de pantalla (si escau).

* Comandes de configuració d'usuaris i permisos al servidor.
* Contingut del fitxer d'exportació (`/etc/exports`) amb justificació de les opcions triades.
* Comanda de muntatge i contingut del fitxer `/etc/fstab` al client.
* Prova final que demostri que el control d'accés basat en permisos UNIX/Linux funciona correctament per als diferents usuaris.

---

## 🔗 Materials de Suport

* Material propi: UD5. AA1. NFS.
* Ruiz, P. (2021). [NFS (parte 1): Instalación en un servidor Ubuntu 20.04 LTS.](https://somebooks.es/nfs-parte-1-instalacion-en-un-servidor-ubuntu-20-04-lts/)
* Ruiz, P. (2021). [NFS (parte 2): Instalación en un cliente Ubuntu 20.04 LTS.](https://somebooks.es/nfs-parte-2-instalacion-en-un-cliente-ubuntu-20-04-lts/)
* Ubuntu Server. [Network File System (NFS).](https://documentation.ubuntu.com/server/how-to/networking/install-nfs/)
---
- [Documentacio](Documentacio.md)
- [Tornar a la pagina principal](../README.md)
