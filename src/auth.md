# Spring Security - OAuth Authentication

<pre><code>
server.port=8080

# Use this pattern to show reduced log width
logging.pattern.console= %d{HH:mm:ss} [%15thread] %msg%n

logging.level.web=TRACE
logging.level.org.springframework.web.client=TRACE

# KeyCloak specific OAuth 2 related properties
spring.security.oauth2.client.registration.keycloak-oidc.provider=keycloak
spring.security.oauth2.client.registration.keycloak-oidc.client-name=bugtracker
spring.security.oauth2.client.registration.keycloak-oidc.client-id=bugtracker
spring.security.oauth2.client.registration.keycloak-oidc.client-secret=VNGk0BtyvKfdwwd8efcjdzr8YisJSEEL
#spring.security.oauth2.client.registration.keycloak-oidc.client-authentication-method=none
spring.security.oauth2.client.registration.keycloak-oidc.authorization-grant-type=authorization_code
spring.security.oauth2.client.registration.keycloak-oidc.scope=openid,profile,email

# This represents the Keycloak Provider (issuer is enough for Spring Boot to know all endpoints)
# Openid configuration - http://127.0.0.1:9090/realms/oauthrealm/.well-known/openid-configuration
spring.security.oauth2.client.provider.keycloak.issuer-uri=http://127.0.0.1:8080/realms/oauth
</code></pre>  

* Here, we see all the configuration which sets up the application to use Spring Security with Keycloak for OAuth 2.0 authentication, enabling detailed logging for web-related operations and configuring the server to run on port 8080.

* There are also a bunch of settings related to logging. TRACE is the most detailed logging level, providing extensive information for debugging purposes.
* Next comes the OAuth 2 related properties specific to Keycloak like client ID, the client secret, the scopes, and the grant type which can be seen in the Clients section of Keycloak server.
* Last is the location of the provider- keycloak. The only thing necessary is the issuer here which can be found in OpenID endpoint configuration link.

**Why?**

Knowing the issuer, the spring boot can calculate the .well-known/openID-configuration, and from there it can find out all of the endpoints of Keycloak server.
