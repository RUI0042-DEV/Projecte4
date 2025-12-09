# Guia Tècnica: Còpia de Seguretat del Perfil d’Usuari a Windows 11 amb Duplicati (FASE 1)
---
## 1. Introducció

Aquesta guia tècnica detalla el procediment per implantar una política de còpies de seguretat segons l’esquema 3-2-1 per a l’equip Windows del director de l’empresa. Es realitzaran còpies locals al disc secundari de l’equip i còpies al núvol (Google Drive) mitjançant l’eina Duplicati.

L’objectiu és assegurar la disponibilitat i integritat de la informació crítica emmagatzemada al perfil de l’usuari, especialment a la carpeta Documents, i validar la capacitat de restauració en cas de pèrdua accidental.

---
## 2. Requisits previs

- Màquina virtual amb Windows 11.
  - Dos discs durs:
  - Disc primari: sistema operatiu.
  - Disc secundari: 10 GB formatat com NTFS (ex: D:\).
  - Compte personal de Google (no escolar) amb accés a Google Drive.
  - Compte d’usuari local.
  - Accés a Internet per a la instal·lació de Duplicati i la sincronització amb el núvol.
> Si no tens el disc secundari creat o assignat, segueix aquests passos abans de continuar:

<img width="862" height="503" alt="image" src="https://github.com/user-attachments/assets/1fa8cd73-b1b0-4998-ad88-0da9009dde1e" />

### Com crear i assignar el disc secundari (si encara no existeix)
- Afegir el disc dur virtual (des de VirtualBox, VMware, etc.):
- Tanca la màquina virtual.
- Vés a Configuració → Emmagatzematge.
- Afegueix un nou disc dur virtual de 10 GB, tipus VDI (VirtualBox) o equivalent.
- Connecta’l a un controlador SATA (no IDE).
- Inicia la màquina virtual.

### Inicialitzar i formatar el disc nou
- Prem Win + R, escriu diskmgmt.msc i prem Enter.
- Localitza el disc nou (apareixerà com a “No asignado”, ex: Disc 1).
- Fes clic dret sobre l’espai “No asignado” → Nuevo volum simple...
- Deixa el tamany per defecte → Següent.
- Assigna la lletra D: → Següent.(No cal justament el D és un exemple.)
- Sistema de fitxers: NTFS
- Marca “Realizar un formato rápido”
- Següent → Finalizar.

### Crear la carpeta de destinació:
Obre l’explorador de fitxers.
Ves a D:\
Crea la carpeta:
``
Backups\Local
``
<img width="787" height="593" alt="image" src="https://github.com/user-attachments/assets/5ec4e0c9-a9b5-4795-8800-eb531a380993" />


## 3. Instal·lació de Duplicati

- 1-Accedeix a la pàgina oficial: https://www.duplicati.com/download.
- 2-Descarrega la versió estable per a Windows.
- 3-Executa el fitxer i segueix l’assistent d’instal·lació:
  - Accepta els termes de llicència.
  - Selecciona "Install for all users".
  - Marca l’opció "Start Duplicati automatically when Windows starts".
- 4- Un cop finalitzada la instal·lació, Duplicati s’executarà com a servei i obrirà automàticament el navegador a:
```bash
http://localhost:8200
```
> Si no s’obre, copia manualment l’adreça anterior al navegador.
<img width="1182" height="698" alt="image" src="https://github.com/user-attachments/assets/5124f25e-6629-473d-822c-d11f4d90cf89" />

## 4. Configuració del backup local (disc secundari)

4.1. Creació del backup

- A la interfície web de Duplicati, fes clic a "Add backup" → "Configure a new backup".
- Pas 1: General
  - Nom del backup: Copia (Aquí pots posar qualsevol nom he posat còpia com a exemple.)
  - Contrassenya: Defineix una contrassenya segura (imprescindible per restaurar).

<img width="1169" height="553" alt="image" src="https://github.com/user-attachments/assets/29cebc20-58d2-4975-9a4c-38114c54f55a" />

- Pas 2: Destinació
  - Tipus: File system (local path or network share)
  - Tipus: File system (local path or network share)
  - Ruta: D:\Backups\Local

<img width="762" height="474" alt="image" src="https://github.com/user-attachments/assets/dc9f7a69-b4e9-4723-b98f-d8f817f58d90" />

- Pas 3: Fonts
  - Clica “My documents” i selecciona:
  - `` C:\Users\director\Documents ``

- Pas 4: Programació
  - Activa "Automatically run backups".
  - Clica “Continue”.
  
<img width="1172" height="552" alt="image" src="https://github.com/user-attachments/assets/d310391a-d6cd-4a92-89fe-73ae39939908" />

- Pas 5: Opcions de copia
  - En aquest apartat podem triar el tipus de còpia de seguretat que volem fer.

<img width="1174" height="628" alt="image" src="https://github.com/user-attachments/assets/a76c7c5f-6639-4df4-a4c9-226e3c6e4507" />

<img width="1176" height="545" alt="image" src="https://github.com/user-attachments/assets/3563127e-cd9c-4cd1-b309-4a83fadd737a" />

> Un cop acabat t'hauria de sortir així.

## 5. Configuració del backup al núvol (Google Drive)

5.1. Preparació a Google Drive

- Inicia sessió amb el teu compte personal de Google.
- Crea una carpeta anomenada: Duplicati_Backups.

<img width="1174" height="661" alt="image" src="https://github.com/user-attachments/assets/d70e1e07-37da-4511-8370-f4a5d8173d1a" />


5.2. Creació del backup a Duplicati

A Duplicati, clica “Add backup” → “Configure a new backup”.

- Pas 1: General
  - Nom: Copia_nuvol ((Aquí pots posar qualsevol nom he posat còpia_nuvol com a exemple.)
  - Contrassenya: La mateixa que en el backup local (recomanat per coherència).

- Pas 2: Destinació
  - Tipus: Google Drive
  - Busca “Google Drive” i inicia sessió amb el teu compte personal.
  - Poses el nom de la ruta.
  - Entras en el link ``https://duplicati-oauth-handler.appspot.com?type=googledrive`` per poder conseguir el AuthID

  <img width="947" height="909" alt="image" src="https://github.com/user-attachments/assets/3f71250a-9cb8-4a85-b9b0-cbfa8977a47c" />

- Pas 3: Fonts
  - Clica “My documents” i selecciona:
  - `` C:\Users\director\Documents ``

- Pas 4: Programació
  - Activa "Automatically run backups".
  - Programa un cop diari a les 18:00.
  - Clica “Continue”.

  <img width="952" height="835" alt="image" src="https://github.com/user-attachments/assets/df05f781-ba0a-46b3-8299-c910b64837ba" />

## 6. Prova de concepte
- Crea arxius de prova a C:\Users\director\Documents (ex: informe_secret.txt).
- Executa manualment els dos backups des de Duplicati (botó “Run now”).
- Verifica:
- El disc D:\Backups\Local conté fitxers .duplicati-*.
- Google Drive → carpeta Duplicati_Backups conté còpies.
- Simula una pèrdua: esborra tot el contingut de Documents.

<img width="932" height="765" alt="image" src="https://github.com/user-attachments/assets/cff15682-bdb4-4142-aa50-5fb709fba9d5" />


## 7. Restauració de dades

7.1. Des del disc local

- A Duplicati, selecciona Backup_Local_Documents_Director → “Restore”.
- Destinació: C:\Users\director\copia
- Clica “Restore” i comprova els arxius.

<img width="921" height="563" alt="image" src="https://github.com/user-attachments/assets/adbdb339-63db-42cb-99f4-ffbfdb8e2423" />

<img width="932" height="820" alt="image" src="https://github.com/user-attachments/assets/35ba6e9a-f46f-45fb-8383-de6e1ae4417f" />

<img width="896" height="827" alt="image" src="https://github.com/user-attachments/assets/23db0adc-bfc8-47c6-ba31-a0520b20eccf" />


7.2. Des de Google Drive

- Selecciona Backup_Cloud_Documents_Director → “Restore”.
- Pot demanar reautenticació a Google.
- Destinació: C:\Users\director\Copia_nuvol
- Verifica la integritat.

<img width="938" height="834" alt="image" src="https://github.com/user-attachments/assets/6c2fccc4-27f9-4373-9109-2ad2db43f8bb" />
