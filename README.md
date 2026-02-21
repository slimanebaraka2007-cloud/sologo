# RezaBook - Plateforme SaaS de Gestion de Rendez-vous

Plateforme de réservation en ligne pensée pour le marché algérien, adaptée aux petites entreprises (salons, barbershops, coachs…).

## Stack Technique

- **Frontend**: Next.js 14 (App Router), Tailwind CSS, shadcn/ui
- **Backend / BDD**: Supabase (PostgreSQL + Auth + Realtime)
- **Langue**: Français
- **Mobile-first, PWA-ready**

## Fonctionnalités MVP

### 1. Authentification Entreprise
- Inscription avec informations d'entreprise
- Connexion sécurisée via Supabase Auth
- Espace isolé par entreprise

### 2. Dashboard Entreprise
- Calendrier des rendez-vous
- Liste des RDV avec statut
- Statistiques simples
- Support du dark mode

### 3. Gestion des Services
- CRUD des services (nom, durée, prix en DA)
- Activation/désactivation

### 4. Gestion des Employés
- Ajouter des employés
- Gérer les horaires de disponibilité
- Associer des services à des employés

### 5. Configuration Horaires
- Jours et créneaux d'ouverture
- Exclusion de jours fériés
- Durée de pause entre RDV

### 6. Lien de Réservation Public
- Page unique par entreprise: `/[slug-entreprise]`
- Affichage des services disponibles
- Choix d'employé (optionnel)
- Calendrier des créneaux libres
- Formulaire simple
- Confirmation instantanée

### 7. Anti-Double-Booking
- Calcul dynamique des créneaux
- Verrouillage temps-réel (Supabase Realtime)

### 8. Notifications
- Email de confirmation
- Email de rappel 24h avant
- Via Resend

### 9. Gestion des Abonnements
- **Starter**: 2 000 DA/mois (1 employé, 50 RDV/mois)
- **Pro**: 4 000 DA/mois (5 employés, illimités)
- **Premium**: 7 000 DA/mois (tout illimité)

## Variables d'Environnement

```env
NEXT_PUBLIC_SUPABASE_URL=your_supabase_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key
RESEND_API_KEY=your_resend_api_key
```

## Installation

```bash
npm install
npm run dev
```

Accédez à l'application sur `http://localhost:3000`

## Structure du Projet

```
src/
├── app/
│   ├── (auth)/
│   ├── (dashboard)/
│   ├── [slug]/
│   ├── api/
│   └── layout.tsx
├── components/
│   ├── auth/
│   ├── dashboard/
│   ├── booking/
│   └── common/
├── lib/
│   ├── supabase.ts
│   ├── utils.ts
│   └── constants.ts
├── hooks/
├── types/
├── styles/
└── services/
```

## Développement

- `npm run dev` - Mode développement
- `npm run build` - Build production
- `npm start` - Serveur production
- `npm run lint` - Lint le code
