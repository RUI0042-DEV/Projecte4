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


### Directoris compartits
> 📍 [Només al servidor]

```bash
sudo mkdir -p /srv/nfs/{dev_projects,admin_tools}

# Propietari: root | Grup: corresponent
sudo chown root:devs /srv/nfs/dev_projects
sudo chown root:admins /srv/nfs/admin_tools

# Permisos: només el grup pot llegir/escriure
sudo chmod 770 /srv/nfs/dev_projects
sudo chmod 770 /srv/nfs/admin_tools
```
### Instal·lació del servidor NFS
> 📍 [Només al servidor]
```bash
sudo apt install nfs-kernel-server -y
```
