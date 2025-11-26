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

