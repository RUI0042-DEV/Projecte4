# 💾 T03: DRP: Imatges de Sistema (Disaster Recovery Plan)

## 📝 Breu Descripció
Aquesta tasca forma part de l'elaboració del **Pla de Recuperació davant Desastres (DRP)** per al client. L'objectiu és garantir la ràpida posada en marxa dels equips de treball (basats en **Zorin OS 18**), creant i restaurant imatges completes del sistema.

El lliurament es divideix en dues parts:
1.  **Anàlisi i Justificació:** Comparativa de solucions de *disk imaging*.
2.  **Guia d'Ús Tècnica:** Manual operatiu per a la creació i restauració d'imatges utilitzant **Rescuezilla** com a prova de concepte (PoC).

---

## 🔎 Fase 1: Anàlisi i Justificació de la Solució Tècnica

La creació d'imatges del sistema és crítica per minimitzar el **Temps de Recuperació (RTO)**. A continuació, es presenta una comparativa de solucions de *disk imaging* per a entorns Linux.

### Comparativa de Solucions de Disk Imaging

| Categoria | Producte | Característiques Destacades | Preu / Llicència |
| :--- | :--- | :--- | :--- |
| **Comercial** | **Acronis Cyber Protect** | Solució unificada (Backup, Antimalware, DRP). Suport multi-plataforma (inclou Linux). Còpies al núvol i recuperació a metall nu (Bare-Metal Recovery). | Subscripció anual (Varia segons mòduls i escala). |
| **Comercial** | **StorageCraft ShadowXafe** | Enfocament en la continuïtat del negoci i la virtualització (recuperació instantània). Emmagatzematge flexible (local, cloud). | Basat en subscripció, preu per màquina virtual o servidor. |
| **Comunitat (Open Source)** | **Clonezilla** | Estàndard de la comunitat. Molt eficient, suporta una àmplia gamma de sistemes de fitxers. Interfície de text simple i enfocada a la funció. | **Gratuït** (Llicència GPL). |
| **Comunitat (Open Source)** | **Rescuezilla** | Fork de Redo Backup. Dissenyat per ser el "Clonezilla fàcil". Interfície gràfica intuïtiva, facilitant l'ús per a personal de manteniment sense coneixements avançats de Linux. | **Gratuït** (Llicència GPLv3). |

### 💡 Solució Proposada i Justificació

**Solució Proposada:** **Rescuezilla** (o **Clonezilla** si es requereix màxima eficiència i es té personal tècnic especialitzat).

**Justificació:**

* **Entorn Linux (Zorin OS):** Totes dues solucions Open Source són natives i altament eficients per a la clonació de sistemes GNU/Linux.
* **RTO Crític:** Aquestes eines permeten la recuperació completa del sistema operatiu, configuració i aplicacions en qüestió de minuts, complint l'objectiu de posada en marxa ràpida.
* **Cost:** El client pot estalviar considerablement en llicències, ja que les solucions de comunitat són **gratuïtes**.
* **Simplicitat (Rescuezilla):** Per al personal de manteniment, **Rescuezilla** ofereix una interfície gràfica amigable, reduint la corba d'aprenentatge i minimitzant errors operatius durant una crisi (RTO).
* **Flexibilitat:** El disc d'arrencada de Rescuezilla/Clonezilla es pot utilitzar per a la clonació a un disc local o per a la restauració des d'una còpia emmagatzemada en una unitat de xarxa.

---

## 🛠️ Fase 2: Guia d'Ús Tècnica amb Rescuezilla (Manual Operatiu)

Aquesta guia detalla els passos per a la creació i restauració d'imatges del sistema **Zorin OS 18** utilitzant **Rescuezilla**.

### 1. Requisits Prèvis

* **Disc d'Arrencada:** Una memòria USB o un CD/DVD amb la imatge ISO de **Rescuezilla**.
* **Destí de Còpia:** Un disc dur extern o una unitat de xarxa (NAS/Servidor de Fitxers) amb prou espai per guardar la imatge del sistema (20 GB o més).
* **Màquina de Prova (Original):** Màquina virtual o física amb Zorin OS 18.
* **Màquina Neta (Destí):** Màquina virtual idèntica (RAM, CPU, mida de disc), però sense SO.

### 2. Creació d'una Imatge Completa del Sistema

L'objectiu és capturar l'estat actual del disc de Zorin OS.

1.  **Arrencar amb Rescuezilla:**
    * Connecteu el medi USB/CD de Rescuezilla a l'equip original.
    * Inicieu l'equip i configureu la BIOS/UEFI per arrencar des de la unitat externa.
    * Seleccioneu "Rescuezilla" i espereu que s'iniciï l'escriptori gràfic.
2.  **Seleccionar 'Backup':**
    * A la pantalla d'inici de Rescuezilla, feu clic a l'opció **"Backup"** .
3.  **Seleccionar el Disc Origen:**
    * Trieu el disc dur que conté el sistema operatiu Zorin OS (normalment `/dev/sda` o similar). Feu clic a "Next".
4.  **Seleccionar les Particions:**
    * Seleccioneu **totes les particions** que formen part del sistema (Partició d'arrencada/EFI, Partició de sistema principal, etc.). Feu clic a "Next".
5.  **Seleccionar el Destí de Còpia:**
    * Trieu el disc dur extern o la unitat de xarxa on es guardarà la imatge. Si és una unitat de xarxa, feu clic a **"Network drive"** i introduïu les credencials (SMB/NFS). Feu clic a "Next".
6.  **Nom i Configuració de la Imatge:**
    * Introduïu un **Nom descriptiu** per a la imatge (Ex: `Zorin_OS_18_Base_2025-11-17`).
    * Seleccioneu el nivell de compressió (es recomana **"High"** per estalviar espai, tot i que trigarà més).
    * Reviseu el resum i premeu **"Yes, start the backup"**.
7.  **Finalització:**
    * Un cop finalitzat el procés, s'avisarà que la imatge s'ha guardat amb èxit. **Apagueu** l'equip.

### 3. Restauració d'Imatge del Sistema (Disaster Recovery)

L'objectiu és restaurar la imatge creada sobre un equip nou/net, simulant la recuperació davant un desastre.

1.  **Preparació de l'Equip Destí:**
    * Connecteu el medi USB/CD de Rescuezilla a la màquina virtual/física idèntica (destí).
    * Assegureu-vos que el disc dur intern estigui present.
    * Arrenqueu amb Rescuezilla.
2.  **Seleccionar 'Restore':**
    * A la pantalla d'inici, feu clic a l'opció **"Restore"** .
3.  **Localitzar la Imatge:**
    * Connecteu el disc dur extern o la unitat de xarxa (si escau, utilitzant **"Network drive"**).
    * Navegueu i seleccioneu el **Nom** de la imatge que voleu restaurar (Ex: `Zorin_OS_18_Base_2025-11-17`). Feu clic a "Next".
4.  **Seleccionar el Disc Destí:**
    * Trieu el disc dur intern de la màquina neta on es restaurarà el sistema. **ATENCIÓ:** Assegureu-vos que és el disc correcte, ja que **s'esborrarà tot el contingut**.
5.  **Revisar i Iniciar la Restauració:**
    * Rescuezilla mostrarà un resum de les particions a restaurar.
    * Si la mida del disc destí és igual o més gran que l'original, el procés serà directe.
    * Confirmeu amb **"Yes, start the restore"**.
6.  **Finalització i Prova:**
    * Un cop completada la restauració, **reinicieu** l'equip.
    * **Traieu** el medi de Rescuezilla.
    * El sistema **Zorin OS 18** hauria d'arrencar amb totes les configuracions, aplicacions i dades exactament igual que a l'equip original.
---
[Tornar a la pagina principal](../README.md)
