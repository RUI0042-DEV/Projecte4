# 📁 Projecte 04: Servidor NFS – Guia d’implementació

> **Client**: DevOptimize Solutions  
> **Objectiu**: Centralitzar el codi font i els actius mitjançant un servidor NFSv3 en un entorn Linux sense autenticació centralitzada.  
> **Entorn**: Ubuntu Server 24.04 (servidor) + Zorin OS 18 (client)  
> **⚠️ Nota sobre la IP**: En aquesta guia s’utilitza `192.168.56.105` com a IP del servidor dins de la xarxa *només amfitrió*, **però aquesta IP pot ser diferent** en el teu entorn. Comprova-la amb `ip a` al servidor i substitueix-la a tot arreu.

---

## 📌 Taula de continguts

- [Fase 1: Preparació de l’entorn](#fase-1-preparació-de-lentorn)
- [Fase 2: Preparació del servidor](#fase-2-preparació-del-servidor)
- [Fase 3: Exportació d’administració – `root_squash`](#fase-3-exportació-dadministració--rootsquash)
- [Fase 4: Exportació de desenvolupament – Control per IP i permisos](#fase-4-exportació-de-desenvolupament--control-per-ip-i-permisos)
- [Fase 5: Muntatge automàtic amb `/etc/fstab`](#fase-5-muntatge-automàtic-amb-etcfstab)
- [Conclusió i recomanacions](#conclusió-i-recomanacions)

---

## Fase 1: Preparació de l’entorn

### Requisits

- **Màquina servidor**: Ubuntu Server 24.04 LTS  
- **Màquina client**: Zorin OS 18  

### Configuració de xarxa

Cada màquina virtual tindrà **dues interfícies de xarxa**:

- **Adaptador 1**: **NAT** → permet l’accés a Internet (per actualitzacions, descàrrega de paquets, etc.).
- **Adaptador 2**: **Només amfitrió** (*Host-only*) → crea una **xarxa privada i aïllada** entre les màquines virtuals (normalment `192.168.56.0/24`).

> ⚠️ **La IP del servidor dins d’aquesta xarxa NO és fixa**. En aquesta guia s’utilitza `192.168.56.105` com a exemple.  
> Per conèixer la teva IP real, executa al servidor:
> ```bash
> ip a
> ```
> Busca la interfície associada a `192.168.56.0/24` (normalment `enp0s8` o similar).

<img width="1114" height="386" alt="image" src="https://github.com/user-attachments/assets/7891778f-a3f4-4984-b760-4ae9b5a4af36" />


### Configuració inicial

En **totes dues màquines**, actualitza el sistema:

```bash
sudo apt update && sudo apt upgrade -y
```

> ✅ Verifica la connectivitat entre client i servidor (des del client):
```bash
ping 192.168.56.105   # substitueix per la teva IP real si cal
```
---

## Fase 2: Preparació del servidor

### Crear grups i usuaris al servidor:

```bash
# Grups
# Grups (📍[Només al servidor – per ara])
sudo groupadd -g 1002 devs
sudo groupadd -g 1004 admins

# Usuaris (📍[Només al servidor – per ara])
sudo useradd -u 1001 -g 1002 -m dev01
sudo useradd -u 1002 -g 1004 -m admin01

# (Opcional) Establir contrasenyes
sudo passwd dev01
sudo passwd admin01
```
<img width="429" height="93" alt="image" src="https://github.com/user-attachments/assets/c9034527-8482-45f2-930e-db390c6d5ee8" />
<img width="545" height="96" alt="image" src="https://github.com/user-attachments/assets/d173fe8d-a226-498f-b77e-9b9155cb3cce" />


### Verificar UID i GID al servidor

```bash
id dev01                # Ex: uid=1001 gid=1002
id admin01              # Ex: uid=1002 gid=1004
getent group devs       # Ex: devs:x:1002:
getent group admins     # Ex: admins:x:1004:
```
<img width="563" height="172" alt="image" src="https://github.com/user-attachments/assets/9a635d3e-628e-4bf7-aea5-e9404c10604f" />


### Crear els mateixos grups i usuaris al client amb els mateixos ID

> 📍 [Només al client]

```bash
# Grups amb GID fix
sudo groupadd -g 1002 devs
sudo groupadd -g 1004 admins

# Usuaris amb UID i GID fixos
sudo useradd -u 1001 -g 1002 -m dev01
sudo useradd -u 1002 -g 1004 -m admin01
```
>  ✅ Important: No cal establir contrasenya al client si no vols, però els usuaris han d’existir.
<img width="398" height="74" alt="image" src="https://github.com/user-attachments/assets/559ae3be-dde4-4410-8a1f-ca0a11b7fd02" />
<img width="478" height="52" alt="image" src="https://github.com/user-attachments/assets/5fb78b96-bb58-4c1f-bd97-31da8d30ce58" />


### Crear directoris compartits
> 📍 [Només al servidor]

```bash
sudo mkdir -p /srv/nfs/{dev_projects,admin_tools}
```
> 🔍 Com comprovar que s’han creat?
```bash
ls -l /srv/nfs/
```
> Sortida esperada (abans de chown):
```
drwxr-xr-x 2 root root ... admin_tools
drwxr-xr-x 2 root root ... dev_projects
```
<img width="575" height="104" alt="image" src="https://github.com/user-attachments/assets/2bd31db0-5d3d-4ab5-8c37-ba29dd01c233" />


### Aplicar propietari i permisos
```bash
# Propietari: root | Grup: corresponent
sudo chown root:devs /srv/nfs/dev_projects
sudo chown root:admins /srv/nfs/admin_tools

# Permisos: només el grup pot llegir/escriure
sudo chmod 770 /srv/nfs/dev_projects
sudo chmod 770 /srv/nfs/admin_tools
```
<img width="625" height="102" alt="image" src="https://github.com/user-attachments/assets/bf2eb8b3-a179-4c92-84cc-a131278fdbb3" />
<img width="535" height="58" alt="image" src="https://github.com/user-attachments/assets/6e802ae1-1390-4c83-bf65-9881c41b6a80" />

> 🔍 Com comprovar que està bé?
```bash
ls -l /srv/nfs/
```
> ✅ Sortida esperada:
```bash
drwxrwx--- 2 root admins ... admin_tools
drwxrwx--- 2 root devs   ... dev_projects
```
<img width="549" height="83" alt="image" src="https://github.com/user-attachments/assets/2712a817-d92c-4487-89d1-119b0de38ffe" />

> ⚠️ Si veus root root, el grup no existeix o el chown ha fallat.

### Instal·lació del servidor NFS
> 📍 [Només al servidor]
```bash
sudo apt install nfs-kernel-server -y
```
---
> 📍 [Només al client] → Instal·lar el client NFS:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install nfs-common -y
```
> ⚠️ Important: Sense nfs-common, el client no podrà muntar recursos NFS, i rebrà errors com:
mount: /mnt/...: opción incorrecta o missing /sbin/mount.nfs.
## Fase 3: Exportació d’administració – root_squash

Prova 1: Comportament per defecte:
> 📍 [Només al servidor] → Edita /etc/exports:
```bash
/srv/nfs/admin_tools 192.168.56.0/24(rw,sync,no_subtree_check)
```
> 💡 L’opció no_subtree_check evita advertències i és la més compatible.
<img width="1092" height="222" alt="image" src="https://github.com/user-attachments/assets/76ae9c8a-0d15-4a85-9a7c-a9cd95e8ae98" />

Recarrega:

```bash
sudo exportfs -ra
```
<img width="1100" height="95" alt="image" src="https://github.com/user-attachments/assets/f2079fdb-56c6-4c9f-b2a3-2a82d2d971b6" />

> 📍 [Només al client]:
```
sudo mkdir -p /mnt/admin_tools
sudo mount 192.168.56.105:/srv/nfs/admin_tools /mnt/admin_tools
```
> 🔍 Important:
No intentis escriure com a root → serà convertit en nobody per root_squash.
No intentis escriure com a dev01 → no pertany al grup admins.
Has d’usar admin01, que pertany al grup admins.

```
# ✅ Correcte: admin01 pertany al grup admins
sudo -u admin01 touch /mnt/admin_tools/test_squash.txt

# ❌ Permís denegat: dev01 no pertany a admins
sudo -u dev01 touch /mnt/admin_tools/test_dev.txt

# ❌ Permís denegat: root → nobody
sudo touch /mnt/admin_tools/test_root.txt
```
📍 [Només al servidor] → Verificar:
```
sudo ls -l /srv/nfs/admin_tools/test_squash.txt
# Sortida esperada: -rw-r--r-- 1 admin01 admins ...
```
> ✅ Això demostra que funciona correctament.

## Fase 4: Exportació de desenvolupament – Control per IP i permisos

> 📍 [Només al servidor] → /etc/exports:
```bash
/srv/nfs/dev_projects 192.168.56.0/24(rw,sync,no_subtree_check) 192.168.56.100(ro,sync,no_subtree_check)
/srv/nfs/admin_tools 192.168.56.0/24(rw,sync,no_subtree_check) 192.168.56.100(ro,sync,no_subtree_check)
```
<img width="1087" height="267" alt="image" src="https://github.com/user-attachments/assets/5b703820-4075-46eb-b0b9-bc48194ef42c" />


Recarregar:
```bash
sudo exportfs -ra
sudo exportfs -v  # Verificar que apareixen les dues regles
```

<img width="1023" height="218" alt="image" src="https://github.com/user-attachments/assets/2a68413c-9eae-4c45-9862-95d8b325589a" />

> 📍 [Client] → Proves:
### Cas 1: IP dins de 192.168.56.0/24 (ex: 192.168.56.101)

```bash
sudo mkdir -p /mnt/dev_projects
sudo mount -t nfs 192.168.56.105:/srv/nfs/dev_projects /mnt/dev_projects
sudo -u dev01 touch /mnt/dev_projects/test_rw.txt  # ✅ Funciona
```
<img width="756" height="91" alt="image" src="https://github.com/user-attachments/assets/0320e605-57ab-4b1b-9cb8-708d0648bcab" />

Comprovació:
```bash
sudo -u dev01 ls -l /mnt/dev_projects/test_rw.txt
```
<img width="651" height="62" alt="image" src="https://github.com/user-attachments/assets/58267285-c49f-49aa-ad16-4eb5301ebceb" />


### Cas 2: Simular IP de consultor (192.168.56.100)
Desmuntar primer
```bash
sudo umount /mnt/dev_projects
# Canvia temporalment la IP
sudo ip addr del 192.168.56.101/24 dev enp0s8
sudo ip addr add 192.168.56.100/24 dev enp0s8
```
<img width="588" height="103" alt="image" src="https://github.com/user-attachments/assets/73a2f90c-903f-488f-8175-bd745a9ea559" />


Torna a muntar
```bash
sudo mount -t nfs 192.168.56.105:/srv/nfs/dev_projects /mnt/dev_projects
sudo -u dev01 touch /mnt/dev_projects/test_ro.txt  # ❌ Només lectura → permís denegat
```

### 🔁 Tornar a l’IP original
> ⚠️ Important: Després de la prova, restaura la IP per continuar.
```bash
# 1. Desmuntar
sudo umount /mnt/dev_projects

# 2. Restaurar IP original
sudo ip addr del 192.168.56.100/24 dev enp0s8
sudo ip addr add 192.168.56.101/24 dev enp0s8

# 3. Verificar
ip a show enp0s8  # Ha de mostrar 192.168.56.101
```

### Cas 3: Accés com a admin01

```bash
sudo mount -t nfs 192.168.56.105:/srv/nfs/dev_projects /mnt/dev_projects # Muntem si no esta muntat
sudo -u admin01 touch /mnt/dev_projects/test_admin.txt  # ❌ No pertany al grup devs → permís denegat
```
<img width="775" height="67" alt="image" src="https://github.com/user-attachments/assets/c3e6b777-e094-40b3-ba92-980fdf4a4be9" />

> 🔐 Això demostra que NFS aplica tant les regles d’IP com els permisos locals.

### Fase 5: Muntatge automàtic amb /etc/fstab
> 📍 [Client] → /etc/fstab:
```bash
192.168.56.105:/srv/nfs/admin_tools /mnt/admin_tools nfs defaults 0 0
192.168.56.105:/srv/nfs/dev_projects /mnt/dev_projects nfs defaults 0 0
```
<img width="917" height="479" alt="image" src="https://github.com/user-attachments/assets/5b18081b-454f-4cae-bf5a-9b21a0848b52" />


Provar:
```bash
sudo mount -a
sudo reboot
mount | grep nfs
df -h
```
<img width="655" height="333" alt="image" src="https://github.com/user-attachments/assets/ec263777-399a-4bd7-9c45-df9bd47e5947" />


> ✅ Els recursos s’han de muntar automàticament.

### Conclusió i recomanacions

✅ Assoliments
- Control d’accés per grup i IP.
- Demostració de root_squash i permisos.
- Muntatge automàtic.

⚠️ Limitacions:
- Gestió manual d’identitats.
- Trànsit sense xifrar.
- Seguretat basada en IP.

🛠 Recomanacions:

- LDAP/FreeIPA per autenticació centralitzada.
- NFSv4 + Kerberos per xifratge.
- Backups diaris de /srv/nfs.
> 💬 Aquesta solució és un pas provisional cap a una infraestructura més robusta.
