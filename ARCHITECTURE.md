# Voltix ERP — Architecture technique

## Vue d'ensemble

```
┌─────────────────────────────────────────────────────────┐
│                    CLIENT (Browser)                      │
│              React 18 + Bootstrap 5 + Axios             │
└──────────────────────────┬──────────────────────────────┘
                           │ HTTP/REST + WebSocket
                           ▼
┌─────────────────────────────────────────────────────────┐
│                   NGINX (Reverse Proxy)                  │
│              SSL/TLS · Load Balancing · Static          │
└──────────────────────────┬──────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│              BACKEND Flask (Gunicorn · 3 workers)        │
│                                                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────┐ │
│  │   Routes    │  │   Models    │  │    Services     │ │
│  │  25 blueprints│  │  Raw SQL  │  │ transaction_mgr │ │
│  └─────────────┘  └─────────────┘  └─────────────────┘ │
│                                                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────┐ │
│  │  JWT Auth   │  │    RBAC     │  │  Socket.IO      │ │
│  │  bcrypt     │  │  Decorator  │  │  Notifications  │ │
│  └─────────────┘  └─────────────┘  └─────────────────┘ │
└──────────────────────────┬──────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                    MySQL 8.0 (InnoDB)                    │
│         utf8mb4 · Foreign Keys · Transactions           │
└─────────────────────────────────────────────────────────┘
```

## Modèle de données — Tables principales

```
customers ──────┐
suppliers ──────┤
                │
products ───────┤──── purchase_items ──── purchases ──── purchase_payments
product_family  │
product_serial  │──── sale_items ──────── sales ───────── sale_payments
                │
users ──────────┤──── roles ──── permissions (RBAC granulaire)
                │
                └──── notifications
                      audit_logs
                      company_settings
                      caisse
                      ticket_mission
                      demande_appro
```

## RBAC — Architecture des permissions

```
Role
 └── permissions[]
      ├── module: "vente"    → actions: ["voir", "creer", "modifier", "supprimer"]
      ├── module: "achat"    → actions: ["voir", "creer", "modifier", "supprimer"]
      ├── module: "produit"  → actions: ["voir", "creer", "modifier", "supprimer"]
      ├── module: "inventaire" → actions: ["voir"]
      ├── module: "tresorerie" → actions: ["voir"]
      ├── module: "caisse"   → actions: ["voir_tableau_bord", "saisir_depense", ...]
      ├── module: "tickets"  → actions: ["creer_demande", "valider", "affecter_bons"]
      └── module: "appro"    → actions: ["voir_mes_demandes", "valider", "livrer"]
```

## Déploiement multi-client

```
                    yezzatech.tn (Serveur OVH)
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
    voltix-client1    voltix-client2   voltix-demo
    .yezzatech.tn    .yezzatech.tn   .yezzatech.tn
         │                │               │
    Container A       Container B     Container C
    Flask:5001        Flask:5002      Flask:5003
    MySQL DB A        MySQL DB B      MySQL DB C
```

## Stack complète

| Couche | Technologie | Version |
|--------|-------------|---------|
| Frontend | React | 18 |
| Frontend | React Router | v6 |
| Frontend | Axios | latest |
| Frontend | Bootstrap | 5 |
| Backend | Python | 3.11 |
| Backend | Flask | 3.0 |
| Backend | Flask-JWT-Extended | 4.7 |
| Backend | bcrypt | 5.0 |
| Backend | Flask-CORS | 4.0 |
| Base de données | MySQL | 8.0 |
| ORM | Raw SQL (PyMySQL) | — |
| Serveur WSGI | Gunicorn | 21.2 |
| Reverse Proxy | Nginx | latest |
| Containerisation | Docker | — |
| Orchestration | Docker Compose | v3.9 |
