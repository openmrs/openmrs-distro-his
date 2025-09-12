FROM openmrs/openmrs-core:2.7.4-amazoncorretto-17

ENV OMRS_EXTRA_LABONFHIR_KEYSTOREPASS="27e3af2b790009d6b8a2fa4eb6c9d8600a92215fa119ff2139e8b29a1bee72a5" \
    OMRS_EXTRA_LABONFHIR_TRUSTSTOREPASS="27e3af2b790009d6b8a2fa4eb6c9d8600a92215fa119ff2139e8b29a1bee72a5" \
    OMRS_EXTRA_LABONFHIR_PASSOWORD="admin"

# Add modules & configurations for the ozone distribution
ADD distro/binaries/openmrs/modules /openmrs/distribution/openmrs_modules
ADD distro/configs/openmrs/initializer_config /openmrs/distribution/openmrs_config
ADD distro/configs/openmrs/properties/fhirproxy.properties /openmrs/data/fhirproxy/config.properties
ADD distro/configs/openmrs/properties/oauth2.properties /openmrs/data/oauth2.properties
ADD bundled-docker/openmrs/tomcat/server.xml /usr/local/tomcat/conf/server.xml
ADD bundled-docker/openmrs/log4j2.xml /openmrs/data/log4j2.xml
ADD distro/binaries/openelis/keystore /etc/openelis-global/keystore
ADD distro/binaries/openelis/truststore /etc/openelis-global/truststore