# T07: Accés Remot - Serveis d’Assistència Remota

**Equip:** Consultoria IT EverPia  
**Data:** Desembre 2025  
**Eina Seleccionada:** RustDesk (Solució Gratuïta i Professional)

---

## Fase 1: Anàlisi Comparativa i Selecció

Hem avaluat les opcions del mercat per trobar una eina que ens permeti donar suport sense costos de llicència elevats ni restriccions d'ús.

### Taula Comparativa

| Criteri | **TeamViewer** | **AnyDesk** | **Chrome Remote Desktop** | **RustDesk** |
| :--- | :--- | :--- | :--- | :--- |
| **Facilitat (Client)** | Molt alta (QuickSupport). | Alta (Portable). | Mitjana (requereix Chrome). | **Alta (Portable i senzill)**. |
| **Model de Preu** | Molt car (Comercial). | Pagament per tècnic. | Gratuït. | **Gratuït i Codi Obert**. |
| **Limitacions** | Talla la sessió si detecta ús comercial. | Restriccions en versió gratuïta. | Cal compte de Google i extensió. | **Cap. Sense límits de temps**. |
| **Instal·lació** | Opcional. | No cal (portable). | Obligatòria (extensió). | **No cal (executable únic)**. |

### Recomanació: RustDesk
Hem triat **RustDesk** perquè és l'alternativa gratuïta definitiva a TeamViewer. Ens permet tenir el control total de les sessions, funciona en tots els sistemes operatius (Windows, macOS, Linux) i és ideal per a EverPia perquè no requereix inversió inicial en llicències.

---

## Fase 2: Guies d’Ús (Documentació Oficial)

### Guia 1: Manual per al Tècnic (Intern d'EverPia)

Com a tècnic, aquest és el teu flux de treball per connectar-te a un client:

1. **Execució:** Obre l'executable de RustDesk al teu ordinador. No cal instal·lar-lo si no vols.

   <img width="1377" height="764" alt="image" src="https://github.com/user-attachments/assets/f3c07c3b-6045-4119-ace4-57a908adc157" />

3. **Connexió:**
   * A la part dreta de la pantalla veuràs un quadre que diu **"Control Remote Desktop"**.
   * Introdueix l'**ID** que t'ha donat el client.

<img width="1358" height="754" alt="Captura de pantalla 2025-12-17 202805" src="https://github.com/user-attachments/assets/121f2a08-2937-4e01-a6f9-1ba397bf69b9" />

   * Prem el botó **"Connect"**.
3. **Autenticació:**
   * El programa et demanarà una **Password**. Demana-li al client o espera que ell accepti la connexió manualment prement "Accept".

<img width="1358" height="773" alt="Captura de pantalla 2025-12-17 202659" src="https://github.com/user-attachments/assets/bf0d18ac-73d9-467e-8e6b-5c6bfeabc7ca" />


4. **Eines de Suport:**
   * **Transferència de fitxers:** Pots arrossegar fitxers directament a la finestra de la sessió.
   * **Xat:** Utilitza la finestra de xat integrada per parlar amb l'usuari si la trucada es talla.
  
<img width="1392" height="792" alt="Captura de pantalla 2025-12-17 202712" src="https://github.com/user-attachments/assets/d685ba2d-7d48-4421-881b-a6f9696416e9" />


> [!IMPORTANT]
> **Seguretat:** En acabar, tanca la finestra de RustDesk. RustDesk genera una contrasenya nova després de cada sessió per seguretat.

---

[Tornar enrere](README.md)
