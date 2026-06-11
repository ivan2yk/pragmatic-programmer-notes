# Chapter 5 - Configuration

## Key Ideas
- Keep values that may change (configuration) outside the app
- Parameterize your app using external configuration
- Common configuration: credentials, logging levels, ports, IP, etc
- Instead of loading configuration as a global data structure, wrap it behind an API — this way decoupling your code from the configuration
- Exposing configuration as an API has these benefits:
  - Multiple apps can share configuration
  - Configuration can be maintained via a UI
  - Configuration becomes dynamic — changes apply at runtime without rebuilding or restarting the application
- In Spring Boot, configuration is externalized via `application.yml` and environment variables — in Kubernetes, via ConfigMaps and Secrets, making it fully dynamic and environment-independent
- In microservices architecture, **Spring Cloud Config Server** is the full implementation of this pattern — configuration lives in a Git repo, the Config Server exposes it as a REST API, and all microservices pull from it at startup or at runtime via `/actuator/refresh` without restarting