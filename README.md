# 🍔 Street Bites - Système de Commande

## 👥 Équipe

**Binôme :**
Krynen & Rousseau

**Formation :** EPSI Bachelor 3 - DevOps  
**Année :** 2024-2025

---

## 📋 Description du projet

Street Bites est une application de commande en ligne pour un food truck. Le système permet aux clients de consulter le menu, passer des commandes depuis leur smartphone, et de suivre l'état de préparation de leur repas.

Le gérant peut gérer son menu (catégories, produits) et suivre les commandes en temps réel depuis une interface dédiée.

---

## 🏗️ Architecture

Ce projet utilise une **architecture microservices** avec :

- **3 services backend indépendants** (Node.js + Express + TypeScript)
- **3 bases de données SQLite** (une par service)
- **1 application web frontend** (React/Vue/Angular/Svelte)
- **1 package partagé** (types TypeScript + utilitaires)

### Services

#### 🍽️ Menu Service
Gère les catégories et les produits du menu.
- Base de données : `menu.db`
- Port : 3001
- Endpoints : `/categories`, `/products`

#### 👤 Customer Service
Gère les clients et leur historique de commandes.
- Base de données : `customer.db`
- Port : 3002
- Endpoints : `/customers`

#### 📦 Order Service
Orchestre les commandes en communiquant avec les autres services.
- Base de données : `order.db`
- Port : 3003
- Endpoints : `/orders`

#### 🌐 Web App
Application frontend pour les clients et le gérant.
- Port : 5173 (Vite) ou 3000 (selon le framework)

---

## 🛠️ Stack Technique

### Backend
- **Runtime :** Node.js
- **Framework :** Express.js
- **Language :** TypeScript
- **ORM :** Prisma
- **Base de données :** SQLite (1 par service)
- **Documentation API :** Swagger/OpenAPI

### Frontend
- **Framework :** [À compléter : React/Vue/Angular/Svelte]
- **Build tool :** Vite (ou autre)
- **Styling :** [À compléter : CSS/Tailwind/MUI/etc.]

### DevOps
- **Gestionnaire de packages :** pnpm (monorepo)
- **Versioning :** Git / GitHub
- **Containerisation :** Docker (optionnel)

---

## 📁 Structure du projet

```
street-bites/
│
├── apps/
│   └── web/                    # Application frontend
│       ├── src/
│       ├── public/
│       └── package.json
│
├── services/
│   ├── menu-service/           # Service de gestion du menu
│   │   ├── src/
│   │   ├── prisma/
│   │   │   └── schema.prisma
│   │   └── package.json
│   │
│   ├── customer-service/       # Service de gestion des clients
│   │   ├── src/
│   │   ├── prisma/
│   │   │   └── schema.prisma
│   │   └── package.json
│   │
│   └── order-service/          # Service de gestion des commandes
│       ├── src/
│       ├── prisma/
│       │   └── schema.prisma
│       └── package.json
│
├── packages/
│   └── shared/                 # Code partagé entre services
│       ├── src/
│       └── package.json
│
├── .gitignore
├── package.json                # Configuration du monorepo
├── pnpm-workspace.yaml         # Configuration workspace pnpm
├── tsconfig.base.json          # Configuration TypeScript de base
├── docker-compose.yml          # (Optionnel) Pour orchestrer les services
└── README.md                   # Ce fichier
```

---

## 🚀 Installation

### Prérequis

- Node.js (v18 ou supérieur)
- pnpm (gestionnaire de packages)

```bash
# Installer pnpm si nécessaire
npm install -g pnpm
```

### Étapes d'installation

1. **Cloner le repository**
```bash
git clone https://github.com/[votre-username]/street-bites.git
cd street-bites
```

2. **Installer les dépendances**
```bash
pnpm install
```

3. **Initialiser les bases de données**
```bash
# Pour chaque service
pnpm --filter menu-service prisma migrate dev
pnpm --filter customer-service prisma migrate dev
pnpm --filter order-service prisma migrate dev

# Générer les clients Prisma
pnpm --filter menu-service prisma generate
pnpm --filter customer-service prisma generate
pnpm --filter order-service prisma generate
```

4. **Populer les bases de données (optionnel)**
```bash
pnpm --filter menu-service prisma db seed
pnpm --filter customer-service prisma db seed
pnpm --filter order-service prisma db seed
```

---

## 🎯 Utilisation

### Démarrer tous les services

```bash
# En mode développement
pnpm dev
```

Ou démarrer chaque service individuellement :

```bash
# Service Menu
pnpm --filter menu-service dev

# Service Customer
pnpm --filter customer-service dev

# Service Order
pnpm --filter order-service dev

# Application Web
pnpm --filter web dev
```

### Accéder aux applications

- **Application Web :** http://localhost:5173 (ou 3000)
- **Menu Service API :** http://localhost:3001
- **Customer Service API :** http://localhost:3002
- **Order Service API :** http://localhost:3003

### Documentation API (Swagger)

- **Menu Service :** http://localhost:3001/api-docs
- **Customer Service :** http://localhost:3002/api-docs
- **Order Service :** http://localhost:3003/api-docs

---

## 📱 Fonctionnalités

### Pour les clients

- ✅ Consulter le menu par catégories
- ✅ Ajouter des produits au panier
- ✅ Passer une commande
- ✅ Suivre l'état de la commande en temps réel
- ✅ Consulter l'historique de commandes

### Pour le gérant (interface cuisine)

- ✅ Voir toutes les commandes en cours
- ✅ Changer le statut des commandes
- ✅ Gérer les catégories (CRUD)
- ✅ Gérer les produits (CRUD)
- ✅ Activer/désactiver la disponibilité des produits

---

## 🔄 Flux de commande

```
1. Client consulte le menu → Menu Service
2. Client ajoute au panier (frontend uniquement)
3. Client valide la commande → Order Service
   ├─ Vérifie le client → Customer Service
   ├─ Vérifie les produits → Menu Service
   └─ Crée la commande
4. Gérant change le statut → Order Service
5. Commande terminée → Order Service enregistre dans l'historique → Customer Service
```

---

## 🧪 Tests

```bash
# Lancer les tests de tous les services
pnpm test

# Tester un service spécifique
pnpm --filter menu-service test
```

---

## 📊 Règles métier implémentées

### Menu Service
- ✅ Prix minimum d'un produit : 0.50€
- ✅ Temps de préparation : entre 1 et 60 minutes
- ✅ Un produit appartient obligatoirement à une catégorie
- ✅ Impossible de supprimer une catégorie contenant des produits
- ✅ Ordre d'affichage via `display_order`

### Customer Service
- ✅ Email unique et valide
- ✅ Téléphone optionnel
- ✅ Impossible de supprimer un client avec un historique

### Order Service
- ✅ Vérification de l'existence du client et des produits
- ✅ Snapshot des prix/noms dans `order_items`
- ✅ Calcul automatique de `estimated_ready_at`
- ✅ Annulation possible uniquement si `pending` ou `confirmed`
- ✅ Enregistrement dans l'historique au passage en `completed`
- ✅ Au moins 1 article avec quantité ≥ 1

### Statuts de commande
```
pending → cancelled
pending → confirmed → cancelled
pending → confirmed → preparing → ready → completed
```

---

## 🐳 Docker (optionnel)

Si tu utilises Docker Compose :

```bash
# Démarrer tous les services
docker-compose up

# Arrêter les services
docker-compose down
```

---

## 📝 Scripts disponibles

```bash
# Développement
pnpm dev              # Démarre tous les services en mode dev

# Build
pnpm build            # Build tous les packages

# Linting
pnpm lint             # Vérifie le code

# Formatage
pnpm format           # Formate le code avec Prettier

# Nettoyage
pnpm clean            # Supprime node_modules et build
```

---

## 🤝 Contribution

Ce projet est un exercice académique dans le cadre du Bachelor 3 à l'EPSI.

---

## 📄 Licence

Projet académique - EPSI 2024-2025

---

## 📞 Contact

Pour toute question concernant le projet :
- [Ton email]
- [Email de ton binôme]

---

**Note :** Les bases de données SQLite sont incluses dans le repository pour faciliter la correction (contrairement aux bonnes pratiques habituelles).
