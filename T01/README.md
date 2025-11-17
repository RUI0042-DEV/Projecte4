# 📂 T01: DRP: Còpies de Seguretat. Estudi Cas Client

## 🌟 Introducció
Aquest document guia el procés d'anàlisi i disseny d'una política de còpies de seguretat (Backup Policy) per a la petita empresa **"Muntatges i Serveis Tècnics SL"**, seguint una dinàmica de treball cooperatiu (Individual, Parelles, Grup).

---

## 🏢 Presentació del Cas Client: Muntatges i Serveis Tècnics SL

### ⚙️ Infraestructura Tècnica
| Element | Contingut Clau | Volum / Característiques |
| :--- | :--- | :--- |
| **Servidor de Fitxers (Ubuntu)** | Documents de Projectes | 300 GB, Creixement moderat |
| | Bases de Dades (Comptabilitat i Clients) | **20 GB, Canvi constant (CRÍTIC)** |
| | Carpetes Personals Usuaris | 100 GB |
| **Equips Clients (10x Win 10/11)** | Informes i Arxius temporals | Treballen majoritàriament al servidor. Alguns usuaris guarden dades locals importants. |
| **Connexió a Internet** | Fibra 600 Mbps | Simètrica (Apte per a Cloud Backup) |

### 🚨 Requisits de Recuperació (RTO/RPO)
| Dades | Temps de Recuperació (RTO) | Pèrdua de Dades Admesa (RPO) | Retenció Mínima |
| :--- | :--- | :--- | :--- |
| **Comptabilitat/Clients (BD)** | **< 4 Hores** | **< 4 Hores** | 1 mes |
| **Altres Dades** | Sense especificar | < 24 Hores | 1 mes |

---

## 🧠 Fase 1: Treball Individual

L'objectiu és realitzar una anàlisi preliminar dels requisits del client abans del debat en equip.

### Qüestions a Resoldre:

1.  **Què Copiar? (Priorització):**
    * Quines són les dades més **crítiques** del servidor? (Justificació basada en RTO/RPO).
    * Cal fer còpia dels 10 equips clients? (Justificació).
2.  **Periodicitat i Tipus de Còpia:**
    * Proposar un calendari bàsic (Diari/Setmanal/Mensual) per a les **dades crítiques**.
    * Quin tipus de còpia s'aplicaria (Completa, Diferencial, Incremental)?
3.  **Mitjans i Ubicació (Regla 3-2-1):**
    * Quin tipus de mitjà utilitzar (Discs durs externs, NAS, Cloud, Cintes)?
    * On es guardarà físicament la còpia més recent per complir la Regla 3-2-1?

---

## 👥 Fase 2: Treball per Parelles

Es comparen les anàlisis individuals i es crea una proposta unificada per a l'esquema **3-2-1**.

### 📝 Proposta Unificada (Taula de Consens)

| Element | Proposta de la Parella | Justificació (RTO/RPO, Cost, Capacitat) |
| :--- | :--- | :--- |
| **Dades Crítiques** | | |
| **Periodicitat (BD)** | | |
| **Tipus de Còpia (BD)** | | |
| **Mitjà 1 (Local)** | | |
| **Mitjà 2 (Extern)** | | |

---

## 🏆 Fase 3: Treball en Grup i Document Final

El grup debat les propostes de les parelles i dissenya la **Política de Còpies de Seguretat Definitiva** per al client, que serà el document final del lliurament.

### Document Final: Política de Còpies de Seguretat

#### 1) Dades Objecte de Còpia
* Definició clara de les dades copiades i la seva freqüència, separant:
    * Servidor: Dades Crítiques (BD) vs. No Crítiques (Documents, Home).
    * Clients: Dades locals dels 10 equips (sí/no i per què).

#### 2) Cronograma Setmanal Detallat
* Calendari setmanal amb la planificació d'execució de les còpies per a cada conjunt de dades.

| Dia | Dades (Ex: BD) | Tipus de còpia | Mitjà (Local / Extern) |
| :--- | :--- | :--- | :--- |
| **Dilluns** | | | |
| **Dimarts** | | | |
| **...** | | | |
| **Diumenge** | | | |

#### 3) Elecció de Mitjans i Ubicació (Regla 3-2-1)
* **3 Còpies:** Descripció de les 3 còpies (Original + 2 backups).
* **2 Mitjans:**
    * **Mitjà 1 (Local):** Especificació del mitjà concret (ex: NAS, Discos USB rotatius) i per a quines dades.
    * **Mitjà 2 (Extern):** Especificació del mitjà (ex: Cloud, Cintes LTO) i el **proveïdor** proposat (ex: Google Cloud, Azure, AWS).
* **1 Fora de Lloc:** Descripció de l'estratègia de còpia externa (física o lògica) i el responsable de la seva gestió.

#### 4) Estratègia de Recuperació (RTO/RPO)
* Explicació detallada de com la política dissenyada **garanteix** el compliment dels requisits crítics:
    * **RPO < 4 hores (BD):** Quina periodicitat de còpia ho permet?
    * **RTO < 4 hores (BD):** Quin mitjà de recuperació ràpida (local) s'utilitza i quin és el procediment per garantir l'accés ràpid a les dades en cas d'un desastre.

---

## 🔗 Materials de Suport

* Moodle 0226 Seguretat Informàtica. RA2.AA3Còpies
* INCIBE. [Copias de seguridad. Una guía de aproximación para el empresario.](https://www.incibe.es/sites/default/files/contenidos/guias/guia_copias_seguridad_empresario_a5_v2.pdf)
* Xataka. **Backup 3-2-1, el método definitivo para mantener a salvo tus datos.** (YouTube): [https://youtu.be/PM_M4Iz6I4o?si=F7DRyDDTZE3hjWn8](https://youtu.be/PM_M4Iz6I4o?si=F7DRyDDTZE3hjWn8)
---
[Tornar a la pagina principal](../README.md)
