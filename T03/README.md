# 💾 T03: DRP: Creació i Restauració d'Imatges de Sistema

## 🎯 Objectiu
Elaborar una part crucial del **Pla de Recuperació davant Desastres (DRP)** del client. L'objectiu és assegurar la **ràpida posada en marxa (RTO)** dels equips de treball (basats en **Zorin OS 18**) mitjançant la creació i restauració d'imatges completes del sistema.

Aquesta tasca individual inclou l'anàlisi de solucions de mercat i la creació d'una guia d'ús amb una eina de codi obert.

---

## 💼 Cas Client: Recuperació Ràpida d'Equips
El client requereix que els treballadors puguin disposar dels seus equips (Zorin OS 18 amb aplicacions preconfigurades) de forma gairebé immediata en cas d'avaria o robatori. La instal·lació manual no és viable a causa del temps que consumeix.

---

## 🔎 Fase 1: Anàlisi i Justificació de la Solució Tècnica

Heu de buscar i comparar eines que permetin el *disk imaging* (creació i restauració de la imatge completa d'un disc).

### 1. Comparativa de Solucions de Disk Imaging
Elaborar una taula comparativa incloent **dos productes comercials** i **dos productes de comunitat (Open Source)**:

| Categoria | Producte | Característiques Destacades | Preu / Llicència |
| :--- | :--- | :--- | :--- |
| Comercial 1 | | | |
| Comercial 2 | | | |
| Comunitat 1 | | | |
| Comunitat 2 | | | |

* **Nota:** La comparativa ha de ser sintètica, no una còpia de les pàgines web.

### 2. Proposta i Justificació
* **Proposta:** Indicar clarament quina solució recomaneu al client.
* **Justificació:** Argumentar la proposta basant-se en els criteris de la comparativa (cost, facilitat d'ús, compatibilitat amb Linux/Zorin OS, i el requisit de RTO ràpid).

---

## 🛠️ Fase 2: Guia d'Ús Tècnica (Manual Operatiu amb Rescuezilla)

Com a prova de concepte (PoC) interna, utilitzareu l'eina de codi obert **Rescuezilla** per crear la guia tècnica operativa per al personal de manteniment.

### Escenari de la PoC
* **Origen:** Màquina virtual amb la imatge base de Zorin OS.
* **Destí:** Una màquina virtual idèntica (mateixos recursos), però amb el disc buit.

### 📝 Contingut de la Guia

La guia ha de ser acurada, pas a pas, i incloure la documentació dels dos processos clau:

#### 1. Crear una Imatge Completa del Sistema
* Detallar el procés d'arrencada amb el medi de Rescuezilla.
* Selecció de la partició/disc d'origen.
* Selecció del destí de la còpia (disc extern o xarxa).
* Configuració i inici de la creació de la imatge.

#### 2. Restaurar la Imatge sobre un Sistema Net
* Detallar el procés de restauració pas a pas.
* Selecció de la imatge (fitxer) a restaurar.
* Selecció acurada del disc destí.
* Inici de la restauració i comprovació final (que l'equip arranqui amb totes les configuracions originals de Zorin OS 18).

* **Requisit:** Incorporar captures de pantalla significatives per facilitar la comprensió del procediment.

---

## 🔗 Materials i Enllaços de Suport

* INCIBE. ¿Ya tienes tu Plan de Recuperación ante Desastres?. [Blog]. Agost 2019.
* Pàgina oficial de Rescuezilla.
---
[Tornar a la pagina principal](../README.md)
