# 🛡️ T02: DPR: Còpies de Seguretat. Cas Pràctic

## 🚀 Breu Descripció

Aquest projecte documenta la **Prova de Concepte (PoC)** i les guies tècniques per a la implementació de la política de còpies de seguretat del client **"Muntatges i Serveis Tècnics SL"**.

S'han utilitzat dues eines clau per cobrir l'esquema de còpia de seguretat **3-2-1**:
* **Duplicati:** Per a equips clients Windows (còpia local i al núvol).
* **Duplicity:** Per al servidor Linux (còpia local i automatització amb Cron).

---

## 💻 Part 1: Còpia de Seguretat dels Equips Clients Windows (Duplicati)

### 🎯 Objectiu
Implementar una solució de còpia de seguretat **3-2-1** per a l'equip del director, que conté informació sensible. S'utilitza **Duplicati** per gestionar les còpies locals i al núvol (Google Drive).

### ⚙️ Entorn de Simulació
* **Sistema Operatiu:** Màquina Virtual amb Windows 11.
* **Discos:** Dos discos (OS + 10 GB per còpies locals).
* **Eina:** [Duplicati](https://duplicati.com/).
* **Destins:** Disc secundari local i Google Drive (Cloud).

### ✅ Procediment i Proves Realitzades

1.  **Instal·lació i Configuració de Duplicati:**
    * Documentació del procés d'instal·lació.
2.  **Configuració de Plans de Còpia:**
    * **Còpia Local:** Cada **hora** al disc secundari de 10 GB.
    * **Còpia al Cloud:** Diàriament a les **18:00 hores** a Google Drive.
3.  **Prova de Dades:**
    * S'han afegit fitxers de prova a la carpeta `Documents` de l'usuari.
4.  **Verificació de Restauració (Local):**
    * Esborrat del contingut de `Documents`.
    * **Restauració reeixida** des del disc secundari.
5.  **Verificació de Restauració (Cloud):**
    * Simulació de pèrdua de còpia local.
    * **Restauració reeixida** des de Google Drive.

---

## 🐧 Part 2: Còpia de Seguretat Servidor Linux (Duplicity + Cron)

### 🎯 Objectiu
Crear una guia tècnica sobre l'ús de **Duplicity** i automatitzar les còpies de seguretat de la carpeta `/home` utilitzant `cron`. Es prioritza la seguretat mantenint la unitat de còpia desmuntada per defecte.

### ⚙️ Entorn de Simulació
* **Sistema Operatiu:** Màquina Virtual amb Ubuntu Server.
* **Discos:** Dos discos (OS + 10 GB per còpies auxiliars).
* **Eina:** `Duplicity`.
* **Programador:** `cron`.

### 📋 Passos de la Guia Tècnica

#### I. Configuració i Proves Manuals
| Pas | Descripció |
| :--- | :--- |
| **1.** | Inicialització i format amb **XFS** del disc auxiliar. Muntatge manual a `/media/backup`. |
| **2.** | Instal·lació de `duplicity`. |
| **3.** | Creació d'usuaris addicionals i 4 fitxers de 10 MB a `/home/usuari`. |
| **4.** | Còpia de seguretat **completa** (`/home` -> `/media/backup`). |
| **5.** | Esborrat de fitxers i **restauració** per comprovar la integritat. |
| **6.** | Addició d'un fitxer de 4 MB i execució de còpia **incremental**. (Observació de l'increment). |
| **7.** | Desmuntatge de la unitat de backup. |

#### II. Automatització de Còpies (Scripts i Cron)

Es creen dos scripts que inclouen el muntatge i desmuntatge de la unitat per motius de seguretat.

| Tipus de Còpia | Script | Programació (Cron com a root) |
| :--- | :--- | :--- |
| **Completa** | `fullbackup.sh` | Diumenges a les **23:00 h** |
| **Incremental** | `incrementalbackup.sh` | De Dilluns a Dissabte a les **23:00 h** |

**Nota de Seguretat:** Els scripts utilitzen la variable d'entorn `PASSPHRASE` per gestionar la contrasenya de xifrat de Duplicity sense intervenció manual: `export PASSPHRASE=contrasenya`.

---

## 📚 Recursos i Documentació

* **Duplicati Web:** [https://duplicati.com/](https://duplicati.com/)
* **Duplicity Man Page:** [http://manpages.ubuntu.com/manpages/trusty/man1/duplicity.1.html](http://manpages.ubuntu.com/manpages/trusty/man1/duplicity.1.html)
* **Programació de Tasques amb Cron:** [https://geekytheory.com/programar-tareas-en-linux-usando-crontab](https://geekytheory.com/programar-tareas-en-linux-usando-crontab)
* **Creació d'Arxius de Prova:**
    * Windows (`fsutil`): [Enllaç](https://waytoit.wordpress.com/2015/03/15/creando-archivos-con-fsutil/)
    * Linux (`dd`): [Enllaç](https://waytoit.wordpress.com/2015/03/21/creando-archivos-de-prueba-en-linux/)
---
[Tornar a la pagina principal](../README.md)
