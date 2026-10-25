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

## Captures d'écran

Les captures ci-dessous utilisent des données fictives.

| | |
|---|---|
| ![Connexion](docs/screenshots/01-connexion.png) **Connexion** | ![Tableau de bord](docs/screenshots/06-tableau-de-bord-etudiant.png) **Tableau de bord (étudiant)** |
| ![Administration](docs/screenshots/03-administration.png) **Administration des utilisateurs et des rôles** | ![Notes](docs/screenshots/07-mes-notes.png) **Relevé de notes (étudiant)** |
| ![Notes enseignant](docs/screenshots/05-notes-enseignant.png) **Saisie des notes (enseignant)** | ![Calendrier](docs/screenshots/08-calendrier.png) **Calendrier** |
| ![Messagerie](docs/screenshots/09-messagerie.png) **Messagerie** | ![Notifications](docs/screenshots/10-notifications.png) **Notifications** |

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

Prérequis : [Docker](https://www.docker.com/) (avec Docker Compose) et [Git](https://git-scm.com/).

1. Cloner le dépôt :
   ```bash
   git clone https://github.com/zineb1904/ENT-SALE.git
   cd ENT-SALE/BACKEND
   ```
2. Créer le fichier `.env` de chaque service à partir du modèle `.env.example` :
   ```bash
   # Linux, macOS ou Git Bash (Windows)
   for s in ms-*; do cp "$s/.env.example" "$s/.env"; done
   ```
   ```powershell
   # PowerShell (Windows)
   Get-ChildItem -Directory ms-* | ForEach-Object { Copy-Item "$($_.FullName)\.env.example" "$($_.FullName)\.env" }
   ```
3. Démarrer l'ensemble (le realm Keycloak `ent-sale` est importé automatiquement) :
   ```bash
   docker compose up --build -d
   ```
4. Attendre 1 à 2 minutes (Keycloak et Cassandra démarrent lentement). Si `ms-upload` ou `ms-download` signalent une erreur Cassandra au premier lancement, redémarrez-les :
   ```bash
   docker compose restart ms-upload ms-download
   ```
5. Ouvrir http://localhost et se connecter avec le compte de démonstration : **admin** / **Admin@123**.
6. Créer les autres comptes (enseignants, étudiants) depuis la page **Administration**.

La console Keycloak est disponible sur http://localhost:8080 (compte `admin` / `admin`).

> **Projet de démonstration.** Les identifiants par défaut (Keycloak, MySQL, MinIO, compte `admin`), le secret du client Keycloak et la clé JWT sont des valeurs de développement : ils doivent être changés avant toute mise en production. Les fichiers `.env` ne doivent jamais être publiés sur GitHub.

## Auteure

**Zineb Afroug** — Développeuse web et applications, Marrakech (Maroc)
