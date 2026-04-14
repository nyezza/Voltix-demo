# Voltix ERP — by Yezza Tech

![Logo](/logo.png)

<div align="center">

**ERP complet pour PME — Ventes · Achats · Stock · Trésorerie · Caisse**

[![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-3.0-black?logo=flask)](https://flask.palletsprojects.com)
[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react)](https://reactjs.org)
[![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?logo=mysql)](https://mysql.com)
[![Docker](https://img.shields.io/badge/Docker-ready-2496ED?logo=docker)](https://docker.com)
[![Version](https://img.shields.io/badge/version-1.2.0-green)]()
[![License](https://img.shields.io/badge/licence-Propriétaire-red)](LICENSE)

[Demo live](https://voltix-demo.yezzatech.tn) · [Vidéo démo](https://vimeo.com/voltix-demo) · [yezzatech.tn](https://www.yezzatech.tn)

> **Dépôt vitrine** — Le code source est propriétaire et non distribué.
> Pour toute demande de licence ou collaboration : [nadhir.y@yezzatech.tn](mailto:nadhir.y@yezzatech.tn)

</div>


## Demo live

| URL | Accès |
|-----|-------|
| [voltix-demo.yezzatech.tn](https://voltix-demo.yezzatech.tn) | Application complète |

**Compte de démonstration :**
```
Email    : demo@yezzatech.tn
Password : Demo@2026!
Rôle     : Admin (accès complet)
```

> Les données de démo sont réinitialisées chaque nuit.

---

## Vidéo démo

[![Voltix ERP Demo](https://vimeo.com/voltix-demo-thumbnail)](https://vimeo.com/voltix-demo)

> Démonstration complète — tous les modules, tous les rôles, scénarios réels.


## Screenshots

> *Screenshots à venir — voir la [vidéo démo](https://vimeo.com/voltix-demo)*


## Modules

| Module | Fonctionnalités |
|--------|----------------|
| **Ventes** | Offres de prix · Commandes clients · Factures · Paiements |
| **Achats** | Demandes de prix · Commandes fournisseurs · Réceptions · Paiements |
| **Stock** | Inventaire temps réel · Alertes seuil · Numéros de série · Import Excel |
| **Clients & Fournisseurs** | Fiches · Codes uniques · Historique · Soldes |
| **Trésorerie & Caisse** | Grand livre · Auxiliaires · Flux consolidés · Rapports |
| **Tickets & Missions** | Interventions terrain · Carnets de bons |
| **Approvisionnement** | Demandes internes · Validation · Livraison |
| **Administration** | RBAC granulaire · Utilisateurs · Paramètres société |


## Architecture

Voir [ARCHITECTURE.md](ARCHITECTURE.md) pour le schéma technique complet.

```
React 18 (SPA)
    ↕ REST API + JWT
Flask 3.0 (Gunicorn · 3 workers)
    ↕ Raw SQL (PyMySQL)
MySQL 8.0 (InnoDB)
```

**Points techniques notables :**
- RBAC granulaire par module et par action — rôles entièrement personnalisables
- Transactions MySQL atomiques via `transaction_manager.py`
- Notifications temps réel via Socket.IO
- Déploiement multi-client automatisé (1 instance Docker par client)
- Audit trail complet sur toutes les opérations


## Déploiement

Voltix est déployé via Docker avec isolation complète par client :

```bash
# Déployer une nouvelle instance client
./deploy_client.sh nom_client 5001 3001

# Résultat automatique :
# Container Docker isolé
# Base MySQL dédiée
# Nginx + SSL Let's Encrypt
# → https://voltix-nom_client.yezzatech.tn
```


## Sécurité

- Authentification **JWT** (access + refresh tokens)
- Mots de passe hashés avec **bcrypt** (coût 12)
- **RBAC** granulaire par module et par action
- CORS restreint aux origines autorisées
- Audit trail complet


## Licence & Contact

Ce logiciel est la propriété exclusive de **Yezza Tech**.
Le code source n'est pas distribué publiquement.

Pour toute demande :
- 📧 [nadhir.y@yezzatech.tn](mailto:nadhir.y@yezzatech.tn)
- 🌐 [yezzatech.tn](https://yezzatech.tn)
- 💼 [linkedin.com/in/nyezza](https://linkedin.com/in/nyezza)


## Autres projets Yezza Tech

| Projet | Description | Lien |
|--------|-------------|------|
| **Medix** | SaaS de gestion de cabinet médical | [github.com/nyezza/medix](https://github.com/nyezza/medix) |
| **Tradix** | Application desktop pour courtiers en poisson | [github.com/nyezza/tradix](https://github.com/nyezza/tradix) |


<div align="center">

Développé par **[Nadhir Yezza](https://www.yezzatech.tn)** · Yezza Tech · Tunisie

**Version 1.2.0** · Avril 2026

</div>
