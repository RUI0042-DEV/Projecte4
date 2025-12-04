# GUIA INTERNA: ACCÉS REMOT VIA SSH
OBJECTIU:
- Permetre la connexió segura a servidors remots mitjançant SSH des de sistemes Linux i Windows.

## 1. CONCEPTES BÀSICS

- SSH és un protocol segur per administrar sistemes remots.
- Xifra la comunicació entre client i servidor.
- Port per defecte: 22.
- Autenticació:
  - Per contrasenya.
  - Per parell de claus (més segur).
 
## 2. PREPARACIÓ DE L'ENTORN

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install ssh
sudo systemctl status ssh
```
<img width="860" height="370" alt="image" src="https://github.com/user-attachments/assets/4fce299e-21c9-4bd4-8d20-81502e41d124" />



Editar el fitxer:

```bash
sudo nano /etc/ssh/sshd_config
```
### Accions recomanades:

> Desactivar accés root:

``
PermitRootLogin no
``

> Restringir usuaris autoritzats:

``
AllowUsers usuari
``

<img width="750" height="600" alt="image" src="https://github.com/user-attachments/assets/656c97fb-401d-4cef-8b4d-f9d637e57f2e" />


> Canviar port (opcional):

``
Port 2222
``
<img width="1113" height="619" alt="image" src="https://github.com/user-attachments/assets/805e86f9-b24f-44ca-a35b-6ea69c79bed6" />


Guardar i reiniciar:

```bash
sudo systemctl restart ssh
```

## TASCA A REALITZAR

- Desactiva l’accés al root (ja explicat amb PermitRootLogin no).
- Crea un nou usuari usuari2:
- posar contraseña l'usuari root

```bash
sudo passwd root
```
Comprova que pot fer login local:

```bash
su - root
```
Creació usuari:

```
sudo useradd -m -s /bin/bash usuari2
sudo passwd usuari2
```
- Configura SSH perquè només es pugui connectar usuari (ja explicat amb AllowUsers usuari).
- Comprova que usuari2 no té connexió:
```bash
ssh usuari2@IP_del_servidor
```
Resultat esperat:

``
Permission denied
``

<img width="900" height="251" alt="image" src="https://github.com/user-attachments/assets/e0ad0185-2857-42cb-8b80-26e243b82078" />

<img width="528" height="88" alt="image" src="https://github.com/user-attachments/assets/cf936215-044f-497d-a708-2d9a0f7858db" />

## 3.Configuració Tunel

> para que serveix un conexio tunel?

Un túnel SSH amb redirecció dinàmica (-D) permet crear un proxy SOCKS al teu equip local. Tot el trànsit que enviïs al proxy es reenvia xifrat a través del servidor SSH. Això és útil per:

- Navegar de manera segura en xarxes insegures.
- Saltar restriccions locals.
- Amagar la teva IP real utilitzant la del servidor SSH.

Anirem al buscador de windows i buscarem "Opciones de internet" i una vegada entrat anirem a conexiones i en configuración de LAN, Opciones avanzadas i posar:

<img width="429" height="583" alt="image" src="https://github.com/user-attachments/assets/6a8faffe-c5a7-46ce-b6d3-38f5fed09573" />

Una vegada acabat obrirem el terminal i farem un:

```bash
ssh -D 9876 usuari@192.168.56.109
```

Un cop fet el ssh anirem al navegador i anirem a descargar el "Wireshark", un cop acabat de descargar el wireshark al obrirem i selecionarem el ethernet.

<img width="848" height="661" alt="image" src="https://github.com/user-attachments/assets/06a2d819-2ad8-4df9-a11b-c255cf194c7d" />

> Amb el Wireshark comprevem que tot el trànsit que generem navegant surt via SSH cap el servidor SSH. Per tant, si tinc el servidor SSH fora de la xarxa local, podré navegar a través d'ell xofrant el trànsit i sense passar pels filtres.

## Inici sessió amb SSH keys

> En el client

```bash
ssh-keygen -t rsa
```

<img width="833" height="404" alt="image" src="https://github.com/user-attachments/assets/afe05439-1a85-49bb-a2ec-2ff4a04b2eb5" />

```bash
ls .\.ssh\
```
<img width="774" height="265" alt="Captura de pantalla 2025-12-03 203907" src="https://github.com/user-attachments/assets/5f5aefe9-4f84-4c48-8b08-cc511a86fb6c" />


```bash
scp .\.ssh\id_rsa.pub usuari@192.168.56.109:
```
<img width="945" height="411" alt="Captura de pantalla 2025-12-03 204022" src="https://github.com/user-attachments/assets/c6ab2770-ab1f-4f6f-a797-a804b16cb3c3" />


> Anirem al servidor

```bash
mkdir .ssh
touch .ssh/authorized_keys
ls
cat id_rsa.pub
cat id_rsa.pub >> .ssh/authorized_keys
```

<img width="1108" height="215" alt="Captura de pantalla 2025-12-03 204200" src="https://github.com/user-attachments/assets/61e7a386-da18-47e6-864f-5e59044176d1" />

Un cop acabat de posar tots als comandas anirem al client i farem:
```bash
ssh usuari@192.168.56.109
```
> Si has fet be tots els pasos no et demanara la contraseña
> 

### Servidor OpenSSH

Obrim a la configuració i busquem "características opcionales".
<img width="1000" height="859" alt="image" src="https://github.com/user-attachments/assets/de709631-e361-4b2d-bc85-8479364e7692" />

- Ver características
- ver las cacterísticas disponibles

I busquem el "Servidor OpenSSH" i al agregem un cop acabat de agregar el servidor OpenSSH i obrirem el powershell com administrador.

```bash
Start-Service sshd
```
I indiquem que el servei arranqui automàticament

```bash
Set-Service -Name sshd -StartupType 'Automatic'
```

<img width="976" height="181" alt="image" src="https://github.com/user-attachments/assets/b1d18195-cbf0-4f60-b351-2435fc837f22" />

Comprovació

Utilitzem aquesta comanda para saber que si el servei actiu i tenim de obrir el powershell com administrador.

```
Get-Service sshd
```

Una vegada acabat afagirem un adaptador mes que sera el adaptador nome anfitrió 

```
New-NetFirewallRule -Name "OpenSSH-Server" -DisplayName "OpenSSH Server (sshd)" -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

I reniciarem el servei

```
Restart-Service sshd
```

I fem un:
```
ipconfig | findstr "IPv4"
```
> para poder saber quin ip tenim para fer el ssh

<img width="560" height="93" alt="Captura de pantalla 2025-12-04 190058" src="https://github.com/user-attachments/assets/30b96e96-399d-4981-8717-655d63d4bbb6" />


```
ssh rui@192.168.56.111
```
<img width="867" height="129" alt="image" src="https://github.com/user-attachments/assets/ccae33b7-f232-4855-82e1-83ff038a4199" />

Un cop conectat en el ssh ens tendria sorti aixi:

<img width="688" height="168" alt="image" src="https://github.com/user-attachments/assets/e16902e0-fd1f-4949-8108-121f1112995f" />

<img width="1116" height="626" alt="image" src="https://github.com/user-attachments/assets/42f78276-d0fe-4605-84c6-008a3e6391c2" />
