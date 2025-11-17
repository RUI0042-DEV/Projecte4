
# 🛡️ DRP: Còpies de Seguretat – Estudi de Cas Client

## 📌 Descripció Breu
Aquest projecte presenta un estudi de cas sobre la implementació d'una estratègia de còpies de seguretat per a l'empresa **Muntatges i Serveis Tècnics SL**, seguint la metodologia **3-2-1 Backup** i els requisits de RTO/RPO.

---

## 📂 Contingut del Projecte
- **Introducció**: Context i objectius del treball.
- **Fase 1**: Treball individual (anàlisi i propostes inicials).
- **Fase 2**: Treball per parelles (consens i disseny d'esquema 3-2-1).
- **Fase 3**: Treball en grup (política final de còpies de seguretat).
- **Document Final**: Política definitiva amb cronograma, mitjans i estratègia de recuperació.

---

## 🖥️ Infraestructura del Cas
- **Servidor Ubuntu** amb:
  - 📐 Documents de Projectes (300 GB)
  - 💾 Bases de Dades Comptabilitat/Clients (20 GB)
  - 🗂️ Carpetes Personals (100 GB)
- **10 equips clients** (Windows 10/11)
- Connexió a Internet: **600 Mbps simètrica**

---

## ✅ Requisits
- **RTO**: Recuperació BD < 4 hores
- **RPO**: Pèrdua màxima BD < 4 hores; resta < 24 hores
- **Retenció**: Històric mínim d’1 mes

---

## 🔐 Estratègia 3-2-1
- **3 còpies** de les dades
- **2 tipus de mitjans** (local + extern)
- **1 còpia fora de lloc** (cloud o ubicació física segura)

---

## 📅 Cronograma Setmanal Exemple
| Dia       | Dades Crítiques       | Tipus de Còpia | Mitjà       |
|-----------|-----------------------|---------------|------------|
| Dilluns   | BD Comptabilitat      | Incremental   | NAS        |
| Dimarts   | Carpetes Personals    | Incremental   | NAS        |
| Dimecres  | Documents Projectes   | Incremental   | NAS        |
| Dijous    | BD Comptabilitat      | Diferencial   | NAS        |
| Divendres | Tot el servidor       | Completa      | Cloud      |
| Dissabte  | BD Comptabilitat      | Incremental   | NAS        |
| Diumenge  | Sense còpia (manteniment) | -         | -          |

---

## 🌐 Recursos
- [https://www.incibe.es](https://www.incibe.es/sites/default/files/contenidos/guias/guia-copias-de-seguridad.pdf)
- [Vídeo explicatiu: Backup 3-2-1](https://youtu.be/PM_M4Iz6I4o?si=F7DRyDDTZE3hjWn8)
Garantir la **continuïtat del negoci** i la **seguretat de les dades** davant incidents, complint amb els requisits establerts.

---
[Tornar a la pagina principal]
