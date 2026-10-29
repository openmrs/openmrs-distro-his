<p align="center">
<a><img src="./configs/keycloak/themes/carbon/login/resources/img/openmrs-his.svg" alt="OpenMRS HIS" width="30%"/></a>
</p>

**OpenMRS HIS** is an enhanced distribution of OpenMRS, **powered by Ozone**, that extends the platform beyond its EMR capabilities to deliver a full **Health Information System (HIS)**.

While OpenMRS lays the groundwork for patient data management and medical record keeping, OpenMRS HIS brings much more by integrating complementary applications: **Odoo for ERP (billing, stock, pharmacy)**, **OpenELIS Global for laboratory management**, and **Ozone Analytics for real-time reporting**; all unified and secured through **SSO** and aligned with the **OpenHIE Product Catalog specification**.

It is also **white-labeled and ready to be rebranded**, with examples already implemented for Odoo and Keycloak matching the OpenMRS style.

The result is a ready-to-use, modular, and scalable HIS that builds on the strengths of OpenMRS while meeting the broader needs of health facilities and national health programs.

---

## Try it online

Demo Environment: https://dev-his.openmrs.org  
Username: jdoe  
Password: password

## HIS services portal

The bundled Docker proxy serves a service portal at the gateway root instead of redirecting directly to the OpenMRS SPA. After starting the generated Docker Compose deployment, open `http://localhost:8081/` to access the service cards.

The portal links to the applications using the default published ports in the bundled configuration:

- OpenMRS EMR: `http://localhost:8081/openmrs/spa/`
- Odoo ERP, billing and inventory: `http://localhost:8069/`
- Keycloak identity and access: `http://localhost:8084/`
- OpenELIS Global laboratory: `http://localhost:8085/` when the optional OpenELIS Compose stack is running

Service status badges refresh automatically. A service that is not enabled or started is marked unavailable; starting the corresponding application makes it accessible from the portal. If you customize published ports, update the links and checks in `proxy/index.html`.

## Running it locally

For a quick start, see **[docs/RUNNING.md](docs/RUNNING.md)**. For gateway routes, proxy assets, and proxy smoke tests, see **[docs/PROXY.md](docs/PROXY.md)**.

The official Ozone start/stop guide is also available [here](https://docs.ozone-his.com/implementers/start-stop/).

---

<br/>

<p align="center">
    <a href="https://docs.ozone-his.com/"><img src="https://raw.githubusercontent.com/ozone-his/.github/refs/heads/main/profile/ozone-logo.png" alt="Ozone" width="20%"/></a>
</p>

<h3 align="center">The Instant HIS</h3>

<p align="center">
    <br/>Engage with the Ozone community and access useful resources below:
</p>

<h3 align="center">
    <a href="https://github.com/ozone-his/">Code</a>&nbsp;&nbsp;&nbsp;&nbsp;•&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://docs.ozone-his.com/">Docs</a>&nbsp;&nbsp;&nbsp;&nbsp;•&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://talk.openmrs.org/c/software/ozone-his/70">Forum</a>&nbsp;&nbsp;&nbsp;&nbsp;•&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://openmrs.slack.com/archives/C02PYQD5D0A">Chat Room</a>
</h3>
