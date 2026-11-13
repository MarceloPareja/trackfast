# Sistema TrackFast - Documentación de Arquitectura

**TrackFast** es una solución distribuida basada en una arquitectura de microservicios orientada a la gestión logística, seguimiento en tiempo real y entrega de paquetes. El sistema integra interfaces móviles y web comunicadas a través de un punto de entrada único hacia servicios desacoplados y persistencia híbrida.

---

## Arquitectura General

El sistema sigue un patrón de **Arquitectura Orientada a Microservicios** orquestados en contenedores, separando la capa de presentación, el enrutamiento de red, la lógica de negocio y el almacenamiento de datos:

```text
[Repartidor] <---> [App Móvil (Kotlin)] ──────┐
                                              ├──> [API Gateway (HTTP :10069, :10070)]
[Administrador] ─┐                            │                   │
                 ├──> [Aplicación Web (React)]┘                   ▼
[Cliente] ───────┘                                 [Orquestación de Microservicios]
                                                                  │
                    ┌─────────────────────────────────────────────┼─────────────────────────────┐
                    ▼                                             ▼                             ▼
        [Base de Datos Auxiliar]                     [Base de Datos Principal]             [API Mapa]
           (MongoDB :27017)                             (PostgreSQL :5432)

```

## Estructura de Carpetas
```
labprest/
├── README.md
├── CONTRIBUTING.md
├── .gitignore
├── .github/
│	├── pull_request_template.md
│	└── ISSUE_TEMPLATE/
│	├── historia.md
│	└── defecto.md
└── docs/
├── 01-inicio/
├── 02-requisitos/
├── 03-modelado/
└── proceso/
```