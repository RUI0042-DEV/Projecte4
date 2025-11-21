# Fase 1: Treball individual

De forma individual, heu de donar resposta a les següents preguntes basant-se en el cas pràctic:

### 1. Què copiar? (Priorització)
Quines són les dades més crítiques del servidor? Cal fer còpia dels 10 equips clients? Justifica-ho.

* **Bases de Dades (Comptabilitat/Clients):** Són les dades més importants perquè contenen la informació de facturació i clients. Sense elles, l'empresa no pot treballar.
* **Documents de Projectes:** També s'han de copiar perquè contenen la documentació tècnica de la feina.
* **Equips Clients (Selecció):** No farem còpia de tot l'equip (S.O. i programes), però sí que és necessari copiar la carpeta **"Documents"**, ja que els tècnics hi guarden informes temporals i arxius importants que no estan al servidor.

### 2. Periodicitat i Tipus de Còpia
Proposa un calendari bàsic per a la setmana i quin tipus de còpia aplicaràs per a les dades crítiques.

| Freqüència / Moment | Dades Afectades | Tipus de Còpia | Justificació Tècnica |
| :--- | :--- | :--- | :--- |
| **Intradiari (Cada 3h)** (Ex: 09:00, 12:00, 15:00) | **Només Bases de Dades** (Comptabilitat/Clients) | **Incremental** (o Dump SQL) | **Objectiu: Complir l'RPO < 4h.** Si hi ha una fallada a les 14:00, es recupera la còpia de les 12:00, perdent només 2h de feina. |
| **Diari (Dl - Dj)** (Nocturn: 22:00h) | **Tot el Servidor** (BD + Projectes + Usuaris) | **Incremental** | **Objectiu: Estalviar espai i temps.** Només es copien els fitxers modificats durant el dia. Garanteix l'RPO de 24h per als documents. |
| **Setmanal (Divendres)** (Cap de setmana) | **Tot el Servidor** | **Completa** | **Objectiu: Facilitar la restauració (RTO).** Tenir una còpia sencera cada setmana fa que, en cas de desastre, no s'hagin de processar infinites còpies incrementals. |
| **Mensual** (1r Dissabte del mes) | **Tot el Servidor** | **Completa** (Retenció llarga) | **Objectiu: Compliment legal/intern.** Aquesta còpia es guarda a part per complir amb el requisit d'historial d'almenys un mes. |

### 3. Mitjans i Ubicació
Quin tipus de mitjà de còpia utilitzaries? On s'hauria de guardar físicament la còpia més recent (Regla 3-2-1).

| Element | Solució Proposada | Justificació Tècnica |
| :--- | :--- | :--- |
| **Mitjà 1 (Còpia Local)** | **NAS (Network Attached Storage)** (Emmagatzematge en xarxa) | **Velocitat de Recuperació (RTO):** Tenir les dades en un dispositiu local permet recuperar arxius esborrats en qüestió de minuts a través de la xarxa local, sense dependre d'Internet. |
| **Mitjà 2 (Còpia Externa)** | **Cloud (Núvol)** (Ex: Azure, AWS S3 o Google Cloud) | **Diversitat de Mitjà:** Complim la regla d'usar tecnologies diferents (disc local vs emmagatzematge d'objectes al núvol). Aprofitant la fibra de 600Mbps, la pujada és ràpida. |
| **Ubicació Física** | **Centre de Dades del Proveïdor Cloud** (Fora de l'empresa) | **Ubicació "Offsite" (Regla de l'1):** Si hi ha un incendi, robatori o inundació a l'oficina de "Muntatges SL" (on estan el Servidor i el NAS), aquesta còpia remota permetrà reconstruir l'empresa des de zero. |

---

# Fase 2: Treball per parelles

**1. Discussió i Consens:** Comparen les seves respostes individuals (Fase 1).
**2. Elaboració d'una Proposta Unificada:** Heu de consensuar i dissenyar el vostre propi Esquema 3-2-1 de Còpies.

| Element | Proposta de la Parella | Justificació |
| :--- | :--- | :--- |
| **Dades Crítiques** | 1. Servidor: Bases de Dades + Projectes. 2. Clients: Carpeta "Documents" local. | Les BD són vitals per facturar (crítiques). Els Projectes són la feina acumulada. La carpeta Documents dels clients s'inclou perquè hi ha risc de pèrdua d'informes únics. |
| **Periodicitat (BD)** | **Intradiària: Cada 3 hores** (Ex: 09:00, 12:00, 15:00, 18:00) | Per complir el requisit de pèrdua màxima de dades (**RPO < 4h**). Si hi ha una avaria just abans d'una còpia, com a molt es perdran 2 h i 59 minuts de feina. |
| **Tipus de Còpia (BD)** | **Incremental** (durant el dia) **Completa** (a la nit) | Les incrementals són molt ràpides (segons) i no "congelen" el servidor mentre la gent treballa. La completa nocturna assegura la integritat. |
| **Mitjà 1 (Local)** | **NAS Synology/QNAP** (Connectat per xarxa Gigabit) | Permet complir l'**RTO** (Temps de recuperació). Si s'esborra un fitxer, es recupera a l'instant des de la xarxa local sense esperar descàrregues d'Internet. |
| **Mitjà 2 (Extern)** | **Emmagatzematge al Núvol (Cloud)** (Encriptat) | Aprofitem la fibra de 600 Mbps. És més segur i automatitzat que portar discs durs físics amunt i avall (evita l'error humà i protegeix contra incendi/robatori a l'oficina). |

---

# Fase 3: Treball en grup
