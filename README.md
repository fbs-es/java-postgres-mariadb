# java-postgres-mariadb-template

Gradle-Multiprojekt (Java 21) mit Dev Container, PostgreSQL und MariaDB.
Basiert auf [devcontainers/templates `java-postgres`](https://github.com/devcontainers/templates/tree/main/src/java-postgres), erweitert um MariaDB.

## Start

- **Dev Container:** Ordner in VS Code öffnen → „Reopen in Container“. App, PostgreSQL und MariaDB starten zusammen.
- **Ohne Dev Container:** nur die Datenbanken starten:

  ```bash
  docker compose -f .devcontainer/docker-compose.yml up -d postgres mariadb
  ```

## Datenbanken

|            | im Dev Container | vom Host         | DB / User / Passwort                             |
| ---------- | ---------------- | ---------------- | ------------------------------------------------ |
| PostgreSQL | `postgres:5432`  | `localhost:5440` | `postgres` / `postgres` / `postgres`             |
| MariaDB    | `mariadb:3306`   | `localhost:3340` | `mariadb` / `mariadb` / `mariadb` (root: `root`) |

Im Container stehen die Werte als Umgebungsvariablen (`POSTGRES_*`, `MARIADB_*`) bereit.
Die JDBC-Treiber für beide Datenbanken sind in jedem Teilprojekt eingebunden.

```bash
psql -h postgres -U postgres          # im Dev Container
mariadb -h mariadb -u mariadb -p      # im Dev Container
```

## Skripte

```bash
./script.sh AufgabeXX 'Beschreibung' [KlassenName]   # neue Aufgabe anlegen (inkl. App.java)
./gradlew checkAllTasks                              # alle Tests
./gradlew listTasks                                  # alle Aufgaben auflisten
```
