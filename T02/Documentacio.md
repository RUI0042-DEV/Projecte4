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
  - Disc secundari: 10 GB.
  - Compte personal de Google (no escolar) amb accés a Google Drive.
  - Compte d’usuari local.
  - Accés a Internet per a la instal·lació de Duplicati i la sincronització amb el núvol.

<img width="862" height="503" alt="image" src="https://github.com/user-attachments/assets/1fa8cd73-b1b0-4998-ad88-0da9009dde1e" />

## 3. Instal·lació de Duplicati

- 1-Accedeix a la pàgina oficial: https://www.duplicati.com/download.
- 2-Descarrega la versió estable per a Windows.
- 3-Executa el fitxer i segueix l’assistent d’instal·lació:
  - Accepta els termes de llicència.
  - Selecciona "Install for all users".
  - Marca l’opció "Start Duplicati automatically when Windows starts".
- Un cop finalitzada la instal·lació, Duplicati s’executarà com a servei i obrirà
- 
