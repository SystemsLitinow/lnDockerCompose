# lnDockerCompose

## Running the lnHumanResources project

docker compose -f docker-compose-lnHumanResources.yml up

### Project URLs

http://localhost:8080 --> Angular Frontend  
http://localhost:1080/roundcube --> Roundcube  
http://localhost:1080/postfixadmin --> Postfixadmin  
localhost:3306 --> MySQL

## Running the lnGardenStore project

docker compose -f docker-compose-lnGardenStore.yml up

### Project URLs

http://localhost:8080 --> Angular Frontend  
http://localhost:8100 --> Ionic Frontend  
http://localhost:1080/roundcube --> Roundcube  
http://localhost:1080/postfixadmin --> Postfixadmin  
http://localhost:9093 --> Prometheus  
http://localhost:3000 --> Grafana  
http://localhost:9093 --> AlertManager  
localhost:3306 --> MySQL

## Running the lnToyStore project

docker compose -f docker-compose-lnToyStore.yml up

### Project URLs

http://localhost:8080 --> Angular Frontend  
http://localhost:8100 --> Ionic Frontend  
http://localhost:1080/roundcube --> Roundcube  
http://localhost:1080/postfixadmin --> Postfixadmin  
http://localhost:9090 --> Prometheus  
http://localhost:3000 --> Grafana  
http://localhost:9093 --> AlertManager  
http://localhost:9080/admin --> Keycloak  
localhost:3306 --> MySQL

## Credentials

Postfixadmin - lnmail@litinow.dev/pass123  
Other credentials: admin/admin

## Docker clean-up

Use the following commands when changing from an app to another app.
It will delete dirt left by the previous running.

```
docker container stop $(docker ps -aq) 2>/dev/null || true
docker container rm $(docker ps -aq) 2>/dev/null || true
docker volume rm $(docker volume ls -q) 2>/dev/null || true
docker network rm $(docker network ls -q | grep -vE 'bridge|host|none') 2>/dev/null || true
```

## Docker clean-up via docker-nuke alias

On Linux, add the following block on the end of the ~/.bashrc file:

```
docker-nuke() {
  docker container stop $(docker ps -aq) 2>/dev/null || true
  docker container rm $(docker ps -aq) 2>/dev/null || true
  docker volume rm $(docker volume ls -q) 2>/dev/null || true
  docker network rm $(docker network ls -q | grep -vE 'bridge|host|none') 2>/dev/null || true
}
```

After this, run the following command:

```
source ~/.bashrc
```

## Docker purge images if needed

```
docker rmi -f $(docker images -aq)
```

## Set Grafana + Prometheus

Access Grafana: http://localhost:3000/login

User/Pass = admin/admin

On the left menu click on:

```
Connections
\_____ Data sources

        \_____ Add data source

                \______ prometheus
                        \______ Connection server url:     http://prometheus:9090
                                                           Click on "Save & Test" button at the bottom of the page
```

### Step-by-Step: Importing Dashboards in Grafana

    Log in to Grafana (http://localhost:3000 by default).

    Look for the "+" (plus) icon in the left sidebar menu.
        It’s between the "Search" (magnifying glass) and "Dashboard" (squares) icons.
        Hovering over it will show the label "Create".

    Click "+" > Select "Import" from the dropdown menu.
        This opens the "Import dashboard" screen.

## Pre-built dashboards

### Grafana supports pre-built dashboards for common metrics:

    Navigate to "+" > Import.

    Use these IDs (or upload JSON):
        JVM Micrometer: 4701
        Spring Boot Stats: 11378
    Select your Prometheus data source.

## Verify Prometheus Scraping

### Ensure Prometheus is collecting metrics from your app:

    Access Prometheus at http://localhost:9090/targets.
    Check the status of your app's metrics endpoint (usually actuator/prometheus).
