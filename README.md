# dzone-oauth2-spring
Companion repo for a [DZone article about Oauth2 and Spring](https://dzone.com/articles/spring-oauth2-resource-servers).

## ***B**ackend **F**or **F**rontend* pattern

What we built here is a SPA frontend talking to an OAuth2 resource server by the intermediate of an OAuth2 BFF. The aim is to follow the latest [Spring Security team recommandations](https://github.com/spring-projects/spring-authorization-server/issues/297#issue-896744390) and use a server-side "confidential" OAuth2 client instead of configuring the SPA as a "public" OAuth2 client.

The frontend uses Angular, but **what we'll to to request the REST API has almost nothing specific to that framework and you should be able to port it to React or Vue without much effort.**

The backend is written with Spring. REST APIs is a stateless resource server (no session for scalability and fault-tolerance).

The BFF is a `spring-cloud-gateway` instance in charge of handeling login and logout OAuth2 flows.

All requests from the frontend to the REST API are proxied by the BFF and are authorized with sessions cookie.

Before forwarding the request to a REST API, the BFF replaces the session cookie with an `Authorization` header containing a `Bearer` access token in session.

## Modules
This repo contains two main folders: 
- `backend` with a Maven multi-module project with everything related to Spring. It is itself split into two sub-modules:
  * `official` depending only on `spring-boot-starter-oauth2-client` and `spring-boot-starter-oauth2-resource-server`
  * `with-c4-soft` which uses `spring-addons-starter-oidc` in addition to "official" starters. We'll see that this greatly reduces the amount of Java code and simplifies security configuration.
- `frontend` with a very simple Angular application authenticating users on the BFF and querying the REST API.

Please refer to the README inside each of this folders for more instructions.

## Prerequisites
Spring applications are written for Java 21. You may use SDK-man if you need to keep older Java versions.

Node and npm are required to build the Angular app.

An authorization server instance is needed. This tutorial is written for Keycloak running with SSL on localhost. You might refer to [this Github repository](https://github.com/ch4mpy/self-signed-certificate-generation) if you need to generate a self-signed SSL certificate and to [this other tutorial](https://github.com/ch4mpy/spring-addons/tree/master/samples/tutorials#prerequisites) for running a basic Keycloak instance on your development machine. 