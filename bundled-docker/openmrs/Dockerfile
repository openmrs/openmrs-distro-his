FROM openmrs/openmrs-core:2.7.4-amazoncorretto-17

ENV OMRS_EXTRA_LABONFHIR_KEYSTOREPASSWORD="27e3af2b790009d6b8a2fa4eb6c9d8600a92215fa119ff2139e8b29a1bee72a5" \
    OMRS_EXTRA_LABONFHIR_TRUSTSTOREPASSWORD="27e3af2b790009d6b8a2fa4eb6c9d8600a92215fa119ff2139e8b29a1bee72a5" \
    OMRS_EXTRA_LABONFHIR_PASSWORD="admin"

USER root

# Remove modules and configuration inherited from the base image.
RUN rm -rf \
    /openmrs/distribution/openmrs_modules \
    /openmrs/distribution/openmrs_config \
  && mkdir -p \
    /openmrs/distribution/openmrs_modules \
    /openmrs/distribution/openmrs_config \
    /openmrs/data/fhirproxy

# Install only the Ozone distribution modules and configuration.
COPY --chown=1001:0 distro/binaries/openmrs/modules/ \
  /openmrs/distribution/openmrs_modules/

COPY --chown=1001:0 distro/configs/openmrs/initializer_config/ \
  /openmrs/distribution/openmrs_config/

# Add application properties.
COPY --chown=1001:0 distro/configs/openmrs/properties/fhirproxy.properties \
  /openmrs/data/fhirproxy/config.properties

COPY --chown=1001:0 distro/configs/openmrs/properties/oauth2.properties \
  /openmrs/data/oauth2.properties

# Replace Tomcat and logging configuration.
COPY bundled-docker/openmrs/tomcat/server.xml \
  /usr/local/tomcat/conf/server.xml

COPY --chown=1001:0 bundled-docker/openmrs/log4j2.xml \
  /openmrs/data/log4j2.xml

USER 1001
