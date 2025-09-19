FROM quay.io/keycloak/keycloak:26.3.4

USER root
# remove TLSv1.0 from disabled algorithms
RUN sed -i 's/TLSv1, //g' /etc/java/java-21-openjdk/java-21-openjdk-21.0.8.0.9-1.el9.aarch64/conf/security/java.security

# Switch back to default Keycloak user
USER 100001

ENTRYPOINT ["/opt/keycloak/bin/kc.sh"]
CMD ["start-dev"]