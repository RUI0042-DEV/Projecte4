# 💻 T05: PoC Accés Remot Segur amb SSH (Guia d'Inducció Tècnica)

## 🎯 Objectiu
Aquesta tasca és una **Prova de Concepte (PoC)** interna essencial per a la nostra operació. L'objectiu és crear la **documentació oficial d'accés remot segur** de la consultora, que rebrà el nou personal tècnic. El focus és el protocol **SSH (Secure Shell)**, l'estàndard industrial per a l'administració segura de servidors (especialment Linux) sense accés físic.

El document ha de garantir que qualsevol tècnic nou sigui **operatiu** amb l'accés remot des del primer dia.

---

## 🔑 La Vostra Missió: Crear la Guia de Connexió Segura

Heu d'utilitzar màquines virtuals (MV) per simular els nostres entorns de treball i documentar de manera impecable el procés de connexió SSH, cobrint els dos sistemes operatius clients que utilitzem.

### 1. 🐧 Escenari Client Linux (Terminal Nativa)
Documenteu el procediment per connectar-se a un servidor remot des d'una estació de treball Linux, cobrint els següents aspectes:

* **Connexió bàsica** amb contrasenya.
* Ús de la sintaxi `ssh -p [port]` per a ports no estàndard.
* Demostració de la **transferència de fitxers** utilitzant `scp` o `sftp`.

### 2. 🪟 Escenari Client Windows (Terminal Moderna)
Documenteu l'accés SSH utilitzant les eines modernes de Windows (PowerShell o Windows Terminal amb client OpenSSH natiu), incloent:

* **Connexió bàsica** amb contrasenya.
* Nota sobre l'ús de l'eina nativa (`ssh`) vs. eines de tercers (ex: PuTTY).

### 3. 🛡️ Estàndard de Seguretat: Accés amb Claus
La part més crítica de la PoC és l'adopció de la nostra política de seguretat: l'accés sense contrasenya mitjançant parells de claus **Pública/Privada**.

* **Generació de Claus:** Explicar el procés per generar el parell de claus RSA (`ssh-keygen`).
* **Implementació de la Clau Pública:** Documentar com copiar la clau pública al servidor remot (`authorized_keys`).
* **Prova de Connexió:** Demostrar que l'accés al servidor és possible **sense contrasenya** un cop la clau està instal·lada.

---

## 📝 Documentació i Lliurament

El document final ha de ser una guia tècnica amb captures de pantalla significatives que resolgui tots els punts anteriors, de manera clara i concisa.

### Punts Clau a Incloure:

* Configuració inicial del servidor (si cal).
* Comandes exactes utilitzades per a cada escenari.
* Gestió de la clau a l'ordinador client.

---

## 🔗 Materials de Suport

* Moodle 0227 Serveis de Xarxa. UD4.AA2 Pràctica SSH
* Vídeo. SSH amb clau pública/privada (link)
---
[GUIA](guia.md)
[Tornar a la pagina principal](../README.md)
