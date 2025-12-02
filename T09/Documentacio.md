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
Creació de grups i usuaris
