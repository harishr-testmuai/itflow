# End-to-end (e2e) tests for web

Browser tests written in plain English for [Kane CLI](https://www.testmuai.com/kane-cli/).

| Test | What it covers |
|---|---|
| `setup_test.md` | The setup wizard from checks to telemetry, then the HTTPS-only login guard |
| `client_test.md` | Logging in and creating a client |

## Run

Start a fresh instance on port 4910:

```bash
docker network create itflow-e2e
docker run -d --name itflow-e2e-db --network itflow-e2e --network-alias itflow-db -e MARIADB_RANDOM_ROOT_PASSWORD=true -e MARIADB_DATABASE=itflow -e MARIADB_USER=itflow -e MARIADB_PASSWORD=itflow-e2e mariadb:10.11.6
docker run -d --name itflow-e2e --platform linux/amd64 --network itflow-e2e -p 4910:8080 -e ITFLOW_URL=localhost:4910 -e ITFLOW_PORT=8080 -e ITFLOW_DB_HOST=itflow-db -e ITFLOW_DB_PASS=itflow-e2e itfloworg/itflow:latest
npm install -g @testmuai/kane-cli
kane-cli login
kane-cli testmd run e2e/kane/setup_test.md
```

`client_test.md` needs a set-up instance that allows HTTP login. On a fresh pair of containers, run:

```bash
docker exec -u apache itflow-e2e php /var/www/localhost/htdocs/scripts/setup_cli.php --host=itflow-db --username=itflow --password=itflow-e2e --database=itflow --base-url=localhost:4910 --locale=en_US --timezone=America/New_York --currency=USD --company-name="Morgan IT Services" --country="United States" --user-name="Alex Morgan" --user-email="alex@example.com" --user-password="ITFlow-Test-2026" --non-interactive
docker exec itflow-e2e sed -i 's/config_https_only = TRUE/config_https_only = FALSE/' /var/www/localhost/htdocs/config.php
kane-cli testmd run e2e/kane/client_test.md
```

Recreate both containers before each run.
