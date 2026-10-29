# Running OpenMRS HIS locally

This guide covers a local development/test deployment using the bundled Docker distribution. It is not a production-hardening guide.

## 1. Prerequisites

Install Git, a JDK and Maven version supported by the Ozone HIS parent project, Docker Engine, and the Docker Compose v2 plugin. Allow enough RAM and disk space for OpenMRS, databases, Odoo, Keycloak, and supporting containers.

Verify the tools:

    git --version
    java -version
    mvn -version
    docker --version
    docker compose version

If Java or Maven reports a version error, follow the versions required by the [Ozone HIS implementer documentation](https://docs.ozone-his.com/).

## 2. Get the source

    git clone https://github.com/Bwongo-Digital-Solutions/openmrs-distro-his.git
    cd openmrs-distro-his

To work on the clinicemr branch:

    git fetch origin
    git switch clinicemr

## 3. Build the distribution

Run Maven from the repository root:

    mvn clean package

The build stages the Docker runtime files under:

    target/distro-his-1.0.0-SNAPSHOT/run/docker/

The first build can take some time while Maven downloads dependencies and packages the distribution.

## 4. Start the bundled services

Change to the generated Docker runtime directory and start the stack:

    cd target/distro-his-1.0.0-SNAPSHOT/run/docker
    docker compose up -d
    docker compose ps

Wait for services to initialize. First startup may take several minutes while images are pulled and databases and OpenMRS modules initialize.

Open the services portal at **http://localhost:8080/**.

Default local service URLs:

| Service | URL | Notes |
| --- | --- | --- |
| HIS services portal | http://localhost:8080/ | Entry point for the bundled applications |
| OpenMRS EMR | http://localhost:8080/openmrs/spa/ | Routed through the proxy |
| Odoo ERP, billing and inventory | http://localhost:8069/ | Requires Odoo to be ready |
| Keycloak | http://localhost:8084/ | Identity and access management |
| OpenELIS Global | http://localhost:8085/ | Optional; start the separate OpenELIS stack below |

Portal status badges refresh periodically. A service may appear unavailable while starting or when it is not running.

## 5. Start OpenELIS Global (optional)

OpenELIS is supplied as a separate Compose stack. From the generated runtime directory containing the OpenELIS Compose file, run:

    docker compose -f docker-compose-openelis.yml up -d
    docker compose -f docker-compose-openelis.yml ps

OpenELIS publishes its web UI on host port 8085 in the supplied configuration.

## 6. Monitor, stop, and restart

Run these from the generated Docker runtime directory:

    # View container status
    docker compose ps

    # Follow logs for all bundled services
    docker compose logs -f

    # Follow logs for one service
    docker compose logs -f openmrs

    # Stop bundled services without deleting persistent data
    docker compose down

    # Start them again
    docker compose up -d

For the optional OpenELIS stack, use the same commands with **-f docker-compose-openelis.yml**, for example:

    docker compose -f docker-compose-openelis.yml logs -f
    docker compose -f docker-compose-openelis.yml down

Do not use **docker compose down -v** unless you deliberately intend to remove named volumes and their persisted data.

## 7. Configuration and troubleshooting

- **Port already in use:** Check ports 8080, 8069, 8084, 8085, 3306, and 5432. Stop the conflicting service or adjust the published port and portal links in **proxy/index.html**.
- **A portal card is unavailable:** Check **docker compose ps** and **docker compose logs <service-name>**. OpenELIS is optional and stays unavailable until its separate stack is running.
- **Containers are still starting:** Wait a few minutes and inspect logs; database initialization and OpenMRS module startup can take time.
- **Portal changes do not appear:** Rebuild with **mvn clean package**, then recreate or restart the proxy container from the generated runtime directory.
- **Configuration and credentials:** Use the generated Compose files and environment configuration for local development. Set unique secrets and passwords for any shared or non-demo deployment; do not expose demo credentials or development ports publicly.

For the supported Ozone start/stop workflow and version-specific requirements, see the [Ozone HIS implementer documentation](https://docs.ozone-his.com/implementers/start-stop/).
