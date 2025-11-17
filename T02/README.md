# 💾 T02: DRP: Còpies de Seguretat. Cas Pràctic (Guies Tècniques)

## 🎯 Objectiu
Implementar les guies tècniques amb **Proves de Concepte (PoC)** basades en la política de còpies de seguretat dissenyada per al client **"Muntatges i Serveis Tècnics SL"**. L'objectiu és qualificar el personal del client per aplicar l'esquema de seguretat **3-2-1** tant en entorns Windows com Linux.

---

## 💻 Part 1: Còpia de Seguretat Clients Windows (Duplicati)

### Escenari de la PoC (Regla 3-2-1)
* **Objecte de Còpia:** Perfil d'usuari (informació important no centralitzada).
* **Medi 1 (Local):** Disc secundari intern de l'equip.
* **Medi 2 (Fora de lloc):** Google Drive (Cloud).
* **Eina:** **Duplicati**.

### Requisits i Procediment a Documentar

1.  **Entorn de Treball:** Màquina Virtual Windows 11 amb dos discos (Sistema Operatiu + Disc secundari de 10 GB per a còpies locals).
2.  **Instal·lació:** Documentar el procés d'instal·lació de **Duplicati**.
3.  **Configuració dels Plans:**
    * **Pla Local:** Programació de còpia cada **hora** al disc secundari.
    * **Pla Cloud:** Programació de còpia diària a les **18:00 hores** a Google Drive (ús de compte extern a l'escola).
4.  **Validació de Funcionament:**
    * Afegir fitxers de prova (especialment a `Documents`) i observar l'execució de les còpies.
5.  **Prova de Restauració Local:** Esborrar contingut de `Documents` i procedir a la **restauració** des del disc secundari.
6.  **Prova de Restauració Cloud:** Comprovar la **restauració** des de la còpia emmagatzemada a Google Drive.

---

## 🐧 Part 2: Còpia de Seguretat Servidor Linux (Duplicity + Cron)

### Escenari de la PoC (Automatització Segura)
* **Objecte de Còpia:** Carpeta de l'usuari principal (`/home`).
* **Medi:** Unitat auxiliar local (disc de 10 GB).
* **Eina:** **Duplicity** (còpies completes i incrementals).
* **Programació:** **Cron** (amb scripts de muntatge/desmuntatge per seguretat).

### Requisits i Procediment a Documentar

#### 1. Configuració i Proves Manuals
1.  **Preparació:** Inicialitzar i formatar el disc de 10 GB en **XFS**. Crear la carpeta `/media/backup` i muntar la unitat.
2.  **Instal·lació:** Instal·lar l'eina **Duplicity**.
3.  **Dades de Prova:** Crear 2 usuaris addicionals. Crear **4 arxius de 10 MB** a la carpeta `/home` de l'usuari principal.
4.  **Còpia Completa:** Executar la còpia de seguretat de `/home`.
5.  **Restore:** Esborrar els arxius de prova i executar el **restore** per validar la recuperació.
6.  **Còpia Incremental:** Afegir un nou arxiu de 4 MB i fer una nova còpia. Observar que s'ha creat un segment **incremental**.
7.  **Desmuntatge:** Desmuntar la unitat de backup.

#### 2. Automatització i Seguretat (Scripts i Cron)
La unitat de backup ha d'estar desmuntada per defecte. Els scripts han d'incloure el muntatge i desmuntatge de la unitat.

8.  **Script Completa (`fullbackup.sh`):**
    * Crear l'script per a la còpia **completa** de `/home`.
    * Utilitzar `export PASSPHRASE=contrasenya` per evitar la interacció manual.
    * Donar permisos d'execució.
9.  **Programació Completa:** Programar l'script al **cron (com a root)**:
    * **Diumenges a les 23:00 h.**
10. **Script Incremental (`incrementalbackup.sh`):**
    * Crear l'script per a les còpies **incrementals**.
    * Utilitzar la mateixa variable `PASSPHRASE`.
    * Donar permisos d'execució.
11. **Programació Incremental:** Programar l'script al **cron (com a root)**:
    * **De dilluns a dissabte a les 23:00 h.**

---

## 🔗 Materials i Enllaços de Suport

* **Duplicati Web:** [https://duplicati.com/](https://duplicati.com/)
* **Duplicity Man Page:** [http://manpages.ubuntu.com/manpages/trusty/man1/duplicity.1.html](http://manpages.ubuntu.com/manpages/trusty/man1/duplicity.1.html)
* **Programació de Tasques amb Cron:** [Enllaç a la guia de cron](https://geekytheory.com/programar-tareas-en-linux-usando-crontab)
* **Creació d'Arxius de Prova:** [Guia de creació d'arxius (fsutil / dd)](https://waytoit.wordpress.com/2015/03/21/creando-archivos-de-prueba-en-linux/)
---
[Tornar a la pagina principal](../README.md)
