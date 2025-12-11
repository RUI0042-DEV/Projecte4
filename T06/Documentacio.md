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
