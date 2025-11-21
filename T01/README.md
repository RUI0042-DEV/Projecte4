# 🛡️ T01: DRP: Còpies de Seguretat. Estudi Cas Client (Treball Cooperatiu)

## 🎯 Objectiu
Analitzar la infraestructura de **"Muntatges i Serveis Tècnics SL"** i dissenyar una política de còpies de seguretat completa que compleixi amb els seus requisits de **Temps de Recuperació (RTO)** i **Pèrdua de Dades Admesa (RPO)**, utilitzant la regla **3-2-1**.

Aquesta tasca segueix una dinàmica de treball progressiva: Individual, Parelles i Consens de Grup.

---

## 🏢 Cas Client: Muntatges i Serveis Tècnics SL

### ⚙️ Infraestructura
| Element | Contingut Crític | RPO / RTO Requisits |
| :--- | :--- | :--- |
| **Servidor de Fitxers (Ubuntu)** | **Bases de Dades (Comptabilitat/Clients)** | RPO < 4 hores, RTO < 4 hores |
| | Documents de Projectes (300 GB) | RPO < 24 hores |
| | Carpetes Personals (100 GB) | RPO < 24 hores |
| **10 Equips Clients (Windows)** | Informes i Arxius temporals importants. | A determinar. |
| **Connexió** | Fibra 600 Mbps Simètrica. | Apte per a solucions Cloud. |

### 🚨 Requisit de Retenció
* Cal mantenir un historial de còpies d'almenys **un mes**.

---

## 🧠 Fase 1: Treball Individual

Heu de respondre de forma individual a les següents qüestions, basant-vos únicament en la informació del cas.

### Qüestions a Resoldre:

1.  **Què Copiar? (Priorització):**
    * Quines són les dades més crítiques del servidor i per què?
    * Cal fer còpia dels 10 equips clients? Justificació de la decisió.
2.  **Periodicitat i Tipus de Còpia:**
    * Proposa un calendari bàsic per a les **dades crítiques (BD)** (Diari / Setmanal / Mensual).
    * Quin tipus de còpia aplicaràs (Completa, Diferencial, Incremental) per a les dades crítiques i amb quina freqüència, per complir amb el RPO de 4 hores.
3.  **Mitjans i Ubicació:**
    * Quin tipus de mitjà utilitzaries per a la còpia local (NAS, Discs durs externs, etc.)?
    * On s'hauria de guardar físicament la còpia més recent per complir amb la **Regla 3-2-1**?

---

## 🤝 Fase 2: Treball per Parelles

La parella ha de debatre les respostes individuals i arribar a un consens per dissenyar l'esquema de còpies definitiu.

### 📝 Elaboració de la Proposta Unificada (Esquema 3-2-1)

| Element | Proposta de la Parella | Justificació (Basada en RTO/RPO, Cost i Seguretat) |
| :--- | :--- | :--- |
| **Dades Crítiques** | | |
| **Periodicitat (BD)** | | |
| **Tipus de Còpia (BD)** | | |
| **Mitjà 1 (Local)** | | |
| **Mitjà 2 (Extern)** | | |

---

## 🏆 Fase 3: Treball en Grup i Política Final

El grup debat les propostes de les parelles i redacta el document final que es presentarà a "Muntatges i Serveis Tècnics SL".

### Document Final: Política de Còpies de Seguretat Definitiva

El document ha de resoldre els següents punts de manera detallada:

#### 1) Dades Objecte de Còpia
* Definició de les dades copiades i la seva freqüència (separant Servidor/Clients i crítiques/no crítiques).

#### 2) Cronograma Setmanal Detallat
Calendari detallat de les còpies per a les dades crítiques i no crítiques.

| Dia | Dades (Ex: BD) | Tipus de còpia | Mitjà (Local / Extern) |
| :--- | :--- | :--- | :--- |
| **Dilluns** | | | |
| **Dimarts** | | | |
| **...** | | | |
| **Diumenge** | | | |

#### 3) Elecció de Mitjans i Ubicació (Regla 3-2-1)
* **Mitjà 1 (Local):** Especificació del mitjà concret (p. ex., NAS, Disc USB) i per a quines dades.
* **Mitjà 2 (Extern):** Especificació del mitjà (p. ex., Cloud, LTO) i el **proveïdor** proposat (p. ex., Azure, Google Cloud).
* **Ubicació Fora de Lloc:** Descripció de la gestió de la còpia externa i el responsable.

#### 4) Estratègia de Recuperació (RTO/RPO)
* Explicació de com la política garanteix el compliment dels requisits crítics:
    * **RPO < 4 hores (BD):** Quina freqüència i quin tipus de còpia ho permet?
    * **RTO < 4 hores (BD):** Quin mitjà de recuperació ràpida s'utilitza (p. ex., la còpia local) i quin és el procediment de restauració.

---

## 🔗 Materials i Enllaços de Suport

* Moodle 0226 Seguretat Informàtica. RA2.AA3Còpies
* INCIBE. Copias de seguridad. Una guía de aproximación para el empresario.
* Xataka. **Backup 3-2-1, el método definitivo para mantener a salvo tus datos. [YouTube](https://youtu.be/PM_M4Iz6I4o?si=F7DRyDDTZE3hjWn8):
---
[Solucio](Solucio.md)
---
[Tornar a la pagina principal](../README.md)
