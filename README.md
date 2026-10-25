# ENT-SALE — Espace Numérique de Travail de l'EST Salé

Application web de type ENT (Espace Numérique de Travail) pour l'École Supérieure de Technologie de Salé : connexion sécurisée, consultation des notes, cours en ligne, calendrier, messagerie et notifications, avec une partie administration pour gérer les comptes et les rôles.

Le projet est construit en **micro-services** (Python / FastAPI) derrière une passerelle **Nginx**, et se lance entièrement avec **Docker Compose**.

## Fonctionnalités

- **Authentification** : connexion via Keycloak, jetons JWT (accès et rafraîchissement)
- **Administration** : création, modification et suppression des utilisateurs, attribution des rôles
- **Notes** : saisie et consultation des notes (examen, contrôle, moyenne), avec coefficient, semestre et année universitaire
- **Cours** : dépôt et téléchargement de fichiers de cours
- **Calendrier** : consultation et gestion des événements
- **Messagerie et notifications** : échange de messages entre utilisateurs

## Architecture

| Service | Rôle | Port |
|---|---|---|
| `nginx` | Passerelle API + pages du frontend | 80 |
| `ms-auth` | Authentification, émission et vérification des JWT | 8001 |
| `ms-messaging` | Messages et notifications | 8002 |
| `ms-upload` | Dépôt des cours | 8004 |
| `ms-download` | Téléchargement des cours | 8005 |
| `ms-admin` | Utilisateurs et rôles | 8006 |
| `ms-calendar` | Calendrier | 8007 |
| `ms-notes` | Notes | 8008 |
| `keycloak` | Gestion des identités | 8080 |
| `mysql` | Base de données relationnelle | 3306 |
| `cassandra` | Base de données NoSQL | 9042 |
| `minio` | Stockage des fichiers | 9000 / 9001 |

Chaque service d'API expose sa documentation interactive Swagger sur `/docs`.

## Technologies

- **Backend** : Python, FastAPI, Pydantic, pytest
- **Frontend** : HTML, CSS, JavaScript
- **Données** : MySQL, Cassandra, MinIO
- **Sécurité** : Keycloak, JWT
- **Déploiement** : Docker, Docker Compose, Nginx

## Lancer le projet

Prérequis : [Docker](https://www.docker.com/) et Docker Compose.

1. Cloner le dépôt :
   ```bash
   git clone https://github.com/zineb1904/ENT-SALE.git
   cd ENT-SALE/BACKEND
   ```
2. Créer un fichier `.env` dans le dossier de chaque service (`ms-auth`, `ms-admin`, `ms-messaging`, `ms-upload`, `ms-download`, `ms-calendar`, `ms-notes`). Les variables disponibles sont dans le fichier `config.py` de chaque service.
3. Démarrer l'ensemble :
   ```bash
   docker compose up --build
   ```
4. Configurer Keycloak sur http://localhost:8080 (realm `ent-sale`, client `ent-client`), puis renseigner `KEYCLOAK_CLIENT_SECRET` dans les fichiers `.env`.
5. Ouvrir http://localhost : l'accueil redirige vers la page de connexion.

> **Projet de démonstration.** Les identifiants par défaut (Keycloak, MySQL, MinIO) et la clé JWT de `ms-auth/config.py` sont des valeurs de développement : ils doivent être changés avant toute mise en production.

## Auteure

**Zineb Afroug** — Développeuse web et applications, Marrakech (Maroc)
