# DatePoll-Dockerized
## Information

*  Project website (https://datepoll.org)
*  API / backend documentation (https://datepoll.org/docs/DatePoll)
*  Other projects
    * [DatePoll-Frontend](https://gitlab.com/DatePoll/DatePoll/datepoll-frontend)
    * [DatePoll-Android](https://gitlab.com/DatePoll/DatePoll/datepoll-android)
    * [DatePoll-Backend](https://gitlab.com/DatePoll/DatePoll/datepoll-backend-php)

## Installation
[Here](https://datepoll.org/docs/DatePoll/installation) is an installation guide for the production version
    
## Updating
[Here](https://datepoll.org/docs/DatePoll/update) you will find an update guide.

## Development
A complete development setup guide can be found [here](https://datepoll.org/docs/DatePoll/devAndBuilding).

## Requirements

Install Docker Engine and the Docker Compose CLI plugin. Verify the plugin with `docker compose version`. The installation and update scripts require root privileges, so the plugin must also be available to the user running those scripts.

The Compose configuration remains `docker-compose.yml`, which `docker compose` discovers automatically. From this repository root:

```sh
docker compose up -d
docker compose ps
docker compose logs -f
docker compose down
```

## DatePoll Dockerized commands cheat sheet

** MAKE SURE TO STAY IN THE ROOT DOCKER COMPOSE PROJECT DIRECTORY**

### Docker Compose installation update
`./scripts/dockerUpdate.sh`

### Frontend update
`./scripts/frontend.sh`

### Backend update
`./scripts/backendUpdate.sh`


## PHP runtime upgrade

`phpDockerfile` uses PHP 8.3.36 and Composer 2.10.3, with both base images pinned by digest. IMAP, PDO MySQL, PDO SQLite, and the other backend extensions are built into the image. This supports the Lumen 11 upgrade in the separate DatePoll-Backend-PHP repository. The MySQL image is pinned to 8.3.0, matching the migrated change.

From this infrastructure repository root, rebuild the PHP web and worker images before deploying the updated backend:

```sh
docker compose build datepoll-php datepoll-worker-php
```

Retain the existing backend, database, Redis, and attachment volumes, then install the backend lockfile and restart PHP and queue workers as part of deployment. The existing code/backend and code/frontend layout is preserved; this checkout is not a monorepo and no sibling source directories are copied into it. The web/worker Docker configuration remains owned by this repository; the backend's self-contained CI Dockerfile lives in its own repository.
