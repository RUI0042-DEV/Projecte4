# Guia Tècnica: Còpia de Seguretat amb Duplicity i Cron en Ubuntu Server (FASE 2)

## 1. Introducció

Aquesta guia detalla com implementar una política de còpies de seguretat en un servidor Linux mitjançant Duplicity, una eina de línia d’ordres que permet fer còpies complertes i incrementals, amb suport per a xifratge i múltiples destinacions.

S’utilitzarà un disc secundari local (/dev/sdb) com a emmagatzematge de còpies, que es muntarà/desmuntarà automàticament en cada execució per raons de seguretat. La programació es farà amb cron, seguint una política de còpies setmanals:

- Diumenge: còpia completa.
- Dilluns a dissabte: còpies incrementals.

## 2. Requisits previs
- Màquina virtual amb Ubuntu Server (22.04 LTS o superior).
- Dos discs durs:
- Disc primari (/dev/sda): sistema operatiu.
- Disc secundari (/dev/sdb): 10 GB, no particionat.
- Accés com a usuari amb permisos sudo.
- Connexió a Internet per instal·lar paquets.

<img width="878" height="501" alt="image" src="https://github.com/user-attachments/assets/f2aa389c-9b0b-44f3-8c2a-894fa2751845" />


## 3. Configuració del disc de còpia
>  Si ja tens el disc formatat i muntat, pots saltar aquesta secció. Si no, segueix aquests passos.

Abans de configurar els discos actualitzarem tota la màquina.

```bash
sudo apt update && sudo apt upgrade -y
```

### 3.1. Crear el punt de muntatge

```bash
sudo mkdir -p /media/backup
```

### 3.2. Particionar i formatar el disc secundari en XFS

> Assegura’t que /dev/sdb és el disc secundari (comprova amb lsblk).

```bash
sudo fdisk /dev/sdb
```
Dins de fdisk, escriu aquests comandaments en ordre:

- n → per crear una nova partició
- p → partició primària (prem Enter per defecte)
- 1 → número de partició (Enter per defecte)
- Enter → primer sector per defecte
- Enter → últim sector (usa tot el disc)
- w → escriu els canvis i surt

 Ara tens la partició /dev/sdb1.

<img width="665" height="430" alt="image" src="https://github.com/user-attachments/assets/6064ee51-9ebe-4dda-98da-7992a396b147" />

## 3.3. Format en XFS

```bash
# Instal·la suport per a XFS (si cal)
sudo apt install xfsprogs -y

# Formata el disc en XFS
sudo mkfs.xfs /dev/sdb1
```
> Si el sistema detecta un sistema de fitxers anterior, respon y quan et pregunti si vols continuar.

## 3.4. Provar el muntatge manual

```bash
# Muntar
sudo mount /dev/sdb1 /media/backup

# Verificar
df -h | grep /media/backup

# Crea un fitxer temporal al disc muntat
sudo touch /media/backup/prova.txt

# Llista el contingut
ls -l /media/backup/

# Desmunta
sudo umount /media/backup

# Verifica que ja no apareix a df
df -h | grep /media/backup
```

<img width="534" height="205" alt="image" src="https://github.com/user-attachments/assets/ee70d564-a2d8-4992-8477-b7f5bef1c353" />

> És normal que no surti res quan el disc està desmuntat. Així ha de ser quan no s’estigui fent cap còpia.

## 4. Instal·lació de Duplicity

```bash
sudo apt install duplicity -y
```

## 5. Preparació de dades de prova

### 5.1. Crear usuaris addicionals

```bash
sudo adduser usuari1
sudo adduser usuari2
```
> (Segueix els passos per establir contrasenya; pots deixar la resta per defecte.)
<img width="642" height="580" alt="image" src="https://github.com/user-attachments/assets/4aa86509-c96b-43c6-810b-1962cfbcbbe7" />

### 5.2. Crear arxius de prova (10 MB)

```bash
cd ~
fallocate -l 10M fitxer1.txt
fallocate -l 10M fitxer2.txt
fallocate -l 10M fitxer3.txt
fallocate -l 10M fitxer4.txt
```

>  En sistemes molt antics on ``fallocate`` no existeixi, utilitza: ``dd if=/dev/zero of=fitxer.txt bs=10M count=1``

## 6. Còpia manual i restauració

### 6.1. Fer còpia completa

```bash
sudo mount /dev/sdb1 /media/backup
sudo PASSPHRASE="micontrasenyasegura" duplicity /home file:///media/backup
sudo umount /media/backup
```
<img width="1016" height="428" alt="image" src="https://github.com/user-attachments/assets/8953dd05-2558-45f5-ba19-19aebb458760" />

### 6.2. Restaurar

```bash
rm ~/fitxer*.txt
sudo mount /dev/sdb1 /media/backup
sudo PASSPHRASE="micontrasenyasegura" duplicity restore file:///media/backup /home --force
sudo umount /media/backup
```

<img width="770" height="101" alt="image" src="https://github.com/user-attachments/assets/57cc1843-03d9-4322-94f4-52465b53613b" />
<img width="1094" height="194" alt="image" src="https://github.com/user-attachments/assets/83736ff3-4841-4268-b405-a544974d44c0" />

### 6.3. Fer còpia incremental

```bash
fallocate -l 4M nou_fitxer.txt
sudo mount /dev/sdb1 /media/backup
sudo PASSPHRASE="micontrasenyasegura" duplicity /home file:///media/backup
sudo umount /media/backup
```

<img width="1073" height="499" alt="image" src="https://github.com/user-attachments/assets/58425d26-e2a5-491b-b6f6-f067e24cf40c" />

## 7. Scripts d’automatització

### 7.1. Script de còpia completa (/usr/local/bin/fullbackup.sh)

```bash
#!/bin/bash
export PASSPHRASE="micontrasenyasegura"
sudo mount /dev/sdb1 /media/backup
sudo duplicity /home file:///media/backup
sudo umount /media/backup
```

<img width="997" height="437" alt="image" src="https://github.com/user-attachments/assets/2f0e785d-8daa-4981-8f8e-8d2e23c25a21" />

```bash

```
> Nota: Els scripts usen la mateixa comanda perquè Duplicity decideix automàticament si és completa o incremental segons l’estat del destí.

### 7.2. Script de còpia incremental

```bash
sudo nano /usr/local/bin/incrementalbackup.sh
```

Contingut:

```bash
#!/bin/bash
export PASSPHRASE="micontrasenyasegura"
sudo mount /dev/sdb1 /media/backup
sudo duplicity /home file:///media/backup
sudo umount /media/backup
```

```bash
sudo chmod +x /usr/local/bin/incrementalbackup.sh
```

> Els dos scripts són iguals, però cron els executa en dies diferents. Duplicity decideix automàticament el tipus de còpia segons l’estat del destí.

## 8. Programació amb cron
