# HIS proxy and service portal

The bundled HIS proxy is the public gateway for the portal, OpenMRS SPA/backend, Odoo, and Keycloak. It is based on the proxy configuration pattern used by NidanEHR, but deliberately keeps only routes for services present in this distribution. Nidan-specific routes for DHIS2, Superset, Orthanc, CIS, and other optional components are not copied into this HIS deployment because those services are not defined in this repository's Compose architecture.

## Components

- `bundled-docker/proxy/default.conf.template`: Nginx gateway routes, security headers, compression, Odoo long-polling/WebSocket support, health endpoint, and friendly upstream error page.
- `proxy/index.html`: portal with links and availability badges for OpenMRS, OpenELIS, Odoo, and Keycloak.
- `proxy/502.html`: user-friendly gateway error page.
- `proxy/assets/favicon.svg`: portal favicon.
- `scripts/docker-compose-bundled.yml.template`: mounts the portal, error page, and assets into the proxy container.
- `pom.xml`: packages the portal and static assets into the generated `run/docker/proxy` directory.

## Routing and optional services

- Portal: `http://localhost:8081/`
- OpenMRS: `http://localhost:8081/openmrs/spa/`
- Odoo: `http://localhost:8069/` (the proxy's dedicated Odoo listener)
- Keycloak: `http://localhost:8084/`
- OpenELIS: `http://localhost:8085/` when the optional OpenELIS Compose stack is running.

OpenELIS remains on its separately published port because the optional OpenELIS Compose file defines its own network. Do not add an upstream hostname to the main gateway unless the OpenELIS services are deliberately attached to a network shared with the gateway. This avoids a route that looks configured but fails with an upstream DNS/connectivity error.

The proxy health endpoint is `http://localhost:8081/healthz`. It checks the gateway process, not the health of every upstream application.

## Build and smoke test

From the repository root:

```bash
mvn -DskipTests clean package
```

Then from the generated runtime directory:

```bash
cd target/distro-his-1.0.0-SNAPSHOT/run/docker
docker compose up -d
docker compose ps
curl -fsS http://localhost:8081/healthz
curl -I http://localhost:8081/
curl -I http://localhost:8081/openmrs/spa/
curl -I http://localhost:8069/
curl -I http://localhost:8084/
```

The final three requests depend on their respective services being started. OpenELIS availability depends on starting its optional Compose stack.

For Nginx diagnostics:

```bash
docker compose logs --tail=200 proxy
docker compose exec proxy nginx -t
```

The Maven build and Docker smoke tests must be run in the deployment environment before treating the changes as runtime-verified.
