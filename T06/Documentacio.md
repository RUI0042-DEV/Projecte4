# Guia Tècnica: Connexió per Escriptori Remot (RDP)

> Tasca T06 – Accés remot amb RDP (Prova de Concepte – PoC)

## 1. Què és RDP?

El Remote Desktop Protocol (RDP) és un protocol propietari de Microsoft que permet accedir gràficament a un ordinador remot com si hi estiguéssim asseguts davant. És la solució estàndard per:

- Administrar servidors Windows.
- Donar suport a usuaris finals amb problemes en aplicacions gràfiques.
- Resoldre errors que només es veuen en entorn gràfic.

> Important: Actualment, sistemes GNU/Linux com Zorin, Ubuntu amb GNOME, o altres distribucions també poden actuar com a clients o servidors RDP, gràcies a integracions com GNOME Remote Desktop o xrdp.

---

## 2. Requisits 

- Windows 11
- Zorin OS
- adaptador de nómes l'amfitrió
- Adaptador Nat

## 3. Configuracio windows

### Pas 1

Obrirem configuración > Sistema > Escritorio remoto i habilitar l'opcio.

<img width="1189" height="677" alt="image" src="https://github.com/user-attachments/assets/1f0ddaf0-b721-4792-bb63-2040c074ab8b" />

<img width="1174" height="654" alt="image" src="https://github.com/user-attachments/assets/24d7e75d-6b27-42e4-a11e-35c73ae6ba1f" />

---

### Pas 2 

Clicarem Usuarios de Escritorio remoto `Agregar` i escriurem:

```
# Nom del dispositiu \ i el nom d'usuari del teu compte
# exemple
PC\RUI
```

<img width="1174" height="727" alt="image" src="https://github.com/user-attachments/assets/ffa413bc-dfd4-4ebb-bf92-de85c80f4c4a" />

---
### Pas 3

Haurem de desactivar el `firewall`, ja que ens bloquejarà l'accés per connectar remotament.

Configuración i buscar firewall seleccionar la primera opció i clicar ``cambiar la configuración de notificaciones`` i desactivar el firewall.

<img width="1179" height="651" alt="image" src="https://github.com/user-attachments/assets/39a02c32-71d4-4b55-987c-ed58e253279e" />

## 4. Conexio de zorin a windows

Obrirem la nostra màquina zorin i buscarem "Remmina", ja que és l'eina que nostres utilitzarem per connectar-nos.

```
# Posarem el nom del nostre pc windows i .local
# exemple 
PC.local
```

<img width="1276" height="800" alt="image" src="https://github.com/user-attachments/assets/b2c46e87-9f11-4270-bb5d-65840db83c2e" />

> Seleccionarem que si i ens demanarà l'usuari i la contrasenya del compte de windows.

<img width="1282" height="795" alt="image" src="https://github.com/user-attachments/assets/25620ba2-4bb6-4650-bced-5c59bece9559" />

> Un cop ja posat la contrasenya i l'usuari ja hauries d'estar connectat.

<img width="1276" height="800" alt="image" src="https://github.com/user-attachments/assets/948f2a7f-b261-4c98-932d-c3ad289f7a81" />

## Configuracio Zorin

Obrirem la configuració (Sistema > Escritorio remoto ) activar les opcions següents:

- Compartición de escritorio
  - Compartición de escritorio
  - COntrol remoto

 - Inicio de sesión remoto
   - Inicio de sesión remoto


### Conecio de windows a Zorin

En el buscador de windows buscarem (Conexión a escritorio remoto).

> Aqui hauras de posar:

```
# El nom del teu dispositiu zorin i .local
#exemple
rui-VirtualBox
```

<img width="1195" height="664" alt="image" src="https://github.com/user-attachments/assets/94c3ce3c-f46a-46a4-b4e6-6c56eb5ec9d8" />

> I ens demanarà les credencials.

<img width="1195" height="664" alt="image" src="https://github.com/user-attachments/assets/7537e2d7-7c1c-4ae4-80a7-628f50fed742" />

> Un cop acabat de posar les credencials ens sortirà un missatge d'avis.

<img width="1188" height="679" alt="image" src="https://github.com/user-attachments/assets/c0911c84-21a2-47a8-96e8-5a9571029931" />

Una vegada ja acceptat el missatge d'avis estaries connectat a la teva màquina.

---

[Tornar enrere](README.md)
