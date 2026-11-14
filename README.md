<h1 align="center">Trackfast</h1>

Trackfast es una solución que permite gestionar las entregas y envíos de paquetes de una organización de manera íntegra, al gestionar la información de clientes, paquetes, envíos, repartidores y permitir un monitoreo en vivo del recorrido y estado del paquete desde su salida de bodega hasta su llegada a manos del cliente. 

## Funcionalidades

Trackfast es un sistema completo que trabaja con todos los involucrados en el proceso de entrega de paquetes.

Para los **supervisores**, este sistema ofrece un alto nivel de gestión para al llevar un registro detallado de los clientes, sus paquetes, y los repartidores encargados. Se pueden asignar entregas a los repartidores, y monitorear sus recorridos en todo momento, recibiendo actualizaciones en los estados de cada paquete en tiempo real, y avisos sobre posibles complicaciones durante la entrega. 

Los **repartidores** reciben una aplicación móvil que les permite recibir asignaciones de entregas, planificar sus recorridos diarios, y verificar las entregas de manera rápida y sencilla mediante el uso de códigos QR para continuar con la siguiente entrega.

Finalmente, los **clientes** podrán visualizar desde su navegador en un mapa en tiempo real el recorrido de su paquete, recibir notificaciones acerca de posibles retrasos o contratiempos que tenga el repartidor y confirmar la entrega de su paquete simplemente al enseñar su código QR al repartidor.

## Equipo de Desarrollo

El equipo de desarrollo involucrado en la creación de TrackFast está compuesto por las siguientes personas, que cumplen cada uno un rol y tienen responsabilidades clave en el proyecto.

| Nombre | Rol | Responsabilidades |
| -- | -- | -- |
| **Gabriel López** | Analista y Desarrollador | Encargado de definir la estructura y funcionamiento del sistema, así como del diseño de las interfaces de usuario. |
| **Mateo Medranda** | Líder y Desarrollador | Encargado de supervisar, organizar  y contribuir al proceso de desarrollo. así como de las reuniones y negociación con el cliente. |
| **Marcelo Pareja** | Tester(QA) y Desarrollador | Encargado del desarrollo del sistema, así como de elaborar y ejecutar casos y escenarios de prueba. |

## Modo de trabajo

### Metodología y reuniones

| Elemento | Valor acordado | Justificación |
| - | - | - |
| **Metdología** | Híbrido Ágil/Incremental | Necesidad de presentar avances constantes, y nivel medio de incertidumbre |
| **Duración de iteración** | 1-2 semanas | Tiempo suficiente para realizar micro-ciclos completos de desarrollo (Sprints) respetando documentación. | 
| **Reuniones** | Cada semana, duración 5-15 min| Reunión de verificación de avances y aceptación/denegación y planificación de proximos Sprints|

### Reglas de trabajo en Repositorio

Revisar el archivo de reglas de contribución [CONTRIBUTING.md ](CONTRIBUTING.md)


## Estructura de Carpetas
```
trackfast/
├── README.md
├── CONTRIBUTING.md
├── .gitignore
├── .github/
│   ├── pull_request_template.md
│   └── ISSUE_TEMPLATE/
│       ├── historia.md
│       ├── defecto.md
│       └── caso_de_prueba.md
└── docs/
    ├── 01-inicio/           # Acta, matriz, riesgos y plan de construcción
    ├── 02-requisitos/       # Requisitos, historias y backlog
    ├── 03-modelado/         # Casos de uso, diagramas editables (.drawio, .puml) y arquitectura
    └── proceso/             # PROCESO.md y actas de retrospectiva
```

## Modelado y Casos de Uso
La documentación y diagramas de modelado del sistema se encuentran detallados en [docs/03-modelado/](docs/03-modelado/README.md):
* **Especificación formal:** [Casos_De_Uso.pdf](docs/03-modelado/Casos_De_Uso.pdf)
* **Diagrama editable Draw.io / diagrams.net:** [Casos_De_Uso_Editable.drawio](docs/03-modelado/Casos_De_Uso_Editable.drawio)
* **Diagrama editable PlantUML:** [casos_de_uso.puml](docs/03-modelado/casos_de_uso.puml)
* **Diagrama exportado:** [Diagrama_Casos_De_Uso_General.pdf](docs/03-modelado/Diagrama_Casos_De_Uso_General.pdf)
* **Detalle de casos de uso y matriz de trazabilidad:** [README de Modelado](docs/03-modelado/README.md)

## Descripción de la Arquitectura

TrackFast es una solución logística distribuida basada en una arquitectura orientada a microservicios. El sistema conecta a múltiples actores (Repartidores, Administradores y Clientes) mediante interfaces especializadas: una aplicación móvil nativa para la operación de campo y una plataforma web para la gestión y seguimiento en vivo. 

Toda la comunicación desde los clientes (Móvil y Web) pasa a través de un **API Gateway** unificado que enruta peticiones HTTP usando tokens JWT para seguridad y JSON para la transferencia de datos. En el backend, los procesos de negocio están desacoplados en microservicios desarrollados en C# con .NET, todos orquestados mediante Kubernetes. El sistema implementa una estrategia de persistencia híbrida: los datos transaccionales del negocio descansan en una base de datos principal relacional (PostgreSQL), mientras que el gran volumen de datos de ubicación para trazar rutas se almacena en una base de datos auxiliar NoSQL (MongoDB).

### Tecnologías: 

| Kotlin | React.js | C# | .NET | Docker |Kubernetes | PostgreSQL | MongoDB |
| - | - | - | - | - | - | - | - |

<style>
  .fila-flex {
    display: flex;
    gap: 10px;
    padding: 20px;
  }

  .fila-flex img {
    flex: 1; /* Hace que todas ocupen el mismo espacio disponible */
    min-width: 0; /* Evita que las imágenes desborden el contenedor */
    height: 100px;
    object-fit: contain;
    border-radius: 6px;
  }
</style>

<div align="center">
    <div class="fila-flex">
    <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/webp/kotlin.webp" width="100" height="100" alt="Kotlin">
    <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/webp/reactjs.webp" width="100" height="100" alt="React JS">
    <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/webp/csharp.webp" width="100" height="100" alt="C#">
    <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/webp/dotnet.webp" width="100" height="100" alt="C#">
    <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/webp/docker.webp" width="100" height="100" alt="Kubernetes">
    <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/webp/kubernetes.webp" width="100" height="100" alt="Kubernetes">
    <img src="https://cdn.jsdelivr.net/gh/homarr-labs/dashboard-icons/webp/postgresql.webp" width="100" height="100" alt="PostgreSQL">
    <img src="https://images.icon-icons.com/2415/PNG/512/mongodb_original_logo_icon_146424.png" width="100" height="100" alt="MongoDB">
    </div>
</div>

## Ecosistema de Microservicios

El núcleo del sistema está compuesto por los siguientes servicios independientes:

* **Microservicio de Autenticación:** Encargado de la seguridad transversal del sistema. Provee el control de acceso, validación de identidades y gestión de roles para proteger la operativa de los demás servicios.
* **Microservicio de Gestión de Información:** Administra los datos centrales del dominio, manejando todo lo relacionado con el registro y perfilado de paquetes, clientes y repartidores.
* **Microservicio de Entregas:** Gestiona la logística operativa, encargándose del flujo de trabajo de los repartidores y los paquetes que tienen asignados para su distribución.
* **Microservicio Mapa:** Responsable del componente geoespacial. Recolecta datos de ubicación en tiempo real, permite la auditoría de rutas y se integra con APIs de mapas externas para la visualización.

Los microservicios se desarrollarán en el lenguage de programación **C#**
