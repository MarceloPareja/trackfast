---
name: PR-02
about: Propuesta sobre la arquitectura del sistema Trackfast
labels: Propuestas
---

### Descripción de la propuesta
Como resultado de una investigación y comparación entre las siguientes arquitecturas:
* Monolito
* Microservicios
* Serverless
* Event-Driven

Se escogió la arquitectura de microservicios para el proyecto *Trackfast*, debido a las ventajas que ofrece y a que se adapta correctamente al contexto y necesidades del proyecto.

### Justificación de la propuesta

#### 1. Características y ventajas de la arquitectura de microservicios

* Al separar en microservicios,  se pueden separar correctamente módulos con comportamientos y características distintas (protocolos de comunicación, consumo de recursos externos, tecnologías) manteniendo la funcionalidad unificada del sistema.

* La tolerancia a fallos de uno de los microservicios, implica que en caso de que una parte de la aplicación falle, otros módulos permanecen disponibles.

* Los microservicios permiten controlar el rendimiento de cada módulo por separado en el servidor, esto nos permitirá aumentar los recursos independientemente para cada uno en caso de necesitarlo.

* La flexibilidad tecnológica implica que se pueden usar diferentes lenguajes y tecnologías para cada servicio si se determina que es necesario, lo que minimiza la complejidad de trabajar en cada servicio.

* En caso de refactorización o cambios por mantenimiento, es más fácil  realizar cambios tanto en la capa de presentación como en la capa de servicios y control



#### 2. Criterios de Brown
Una de las razones para escoger la arquitectura es que cumple con los criterios establecidos por Brown para elegir arquitectura. Estos criterios indican que la arquitectura escogida debe ser compatible con:

| **Criterio del proyecto** | **Cumplimiento de la arquitectura escogida** |
| - | - |
|**1.-Requisitos Funcionales**|Dentro de la aplicación, existen funcionalidades relacionadas entre sí, pero con una lógica y comportamientos distintos. La arquitectura de microservicios permite tener una correcta separación de estas funciones. En el caso de las funciones de gestión de información y monitoreo especialmente, aunque usen la misma información, ambas funcionalidades manejan comportamientos y flujos muy distintos.|
|**2.-Requisitos No Funcionales**|Entre los requerimientos de calidad definidos, se hace énfasis claro en la disponibilidad y eficiencia del sistema, así como la tolerancia a fallos, ya que se busca minimizar interrupciones en el servicio a los clientes. Los microservicios permiten cumplir con esas características adecuadamente y con menor complejidad de implementación.|
|**3.-Restricciones del proyecto**|Si bien el equipo de desarrollo es pequeño, y los plazos son cortos, estas limitaciones se pueden superar mediante el uso de herramientas de inteligencia artificial, especialmente la parte de codificación que es la más extensa al desarrollar. Paradigmas como SDD *(Spec-Driven-Development)* mediante el uso de herramientas como `Github Spec-Kit` facilitan un desarrollo fluido, minimizando errores y permitiendo enfocarse en aspectos operativos del sistema como el despliegue.|
|**4.-Principios de desarrollo**|El equipo tiene experiencia previa trabajando con microservicios, y manejan correctamente principios y patrones de diseño que pueden contribuir a acelerar el proceso de desarrollo.|

### Razones de descarte de otras arquitecturas

| **Arquitectura** | **Motivo de Descarte** |
| - | - |
|**Monolítica**| Aunque es el enfoque recomendado para aplicaciones de pequeña y mediana escala y ofrece mayor simplicidad en desarrollo, el combinar varios protocolos y tecnologías complica el desarrollo a largo plazo. Además, el proyecto es altamente susceptible a cambios, por lo que trabajar en un monolito implicaría cambios en cadena para implementar una modificación ligera. Por último, el tener todo el sistema en un único punto de falla implica que si un módulo falla, todo el sistema cae y el servicio se interrumpe. |
|**Serverless**| Esta arquitectura se descartó rápidamente debido a la cantidad excesiva de recursos en la nube que demanda, los cuales no pueden ser costeados debido al alto costo. Además, la dependencia excesiva de proveedores implica que un fallo de su parte compromete todo el sistema.|
|**Orientada a eventos**| Esta arquitectura se descartó ya que se manejan datos y reglas de negocio estructuradas y complejas. Si bien el enfoque de mensajería es válido y se puede usar en la arquitectura escogida para enlazar ciertos microservicios, la implementación de seguridad en cada canal es sumamente compleja, y el contexto del sistema no se acomoda a las soluciones que ofrece esta arquitectura.|

### Descripción de resultados de la propuesta

La arquitectura propuesta se documenta en diagramas que siguen el estándar **C4** propuesto por Simon Brown, quien maneja 4 niveles de abstracción para la arquitectura de un sistema:
|**Nivel**|**Diagrama**|**Contenido**|
| - | - |-|
|**C1** |**Diagrama de sistema o contexto**|Muestra el sistema que ofrece una funcionalidad al usuario junto con otros sistemas externos de los que depende.|
|**C2**|**Diagrama de contenedores**|Muestra unidades funcionales desplegables o módulos, que pueden contener código o datos, junto con sus responsabilidades y tecnologías usadas.|
|**C3**|**Diagrama de componentes**|Muestra componentes que manejan funcionalidades específicas a nivel de código, dentro de un contenedor.|
|**C4**|**Diagrama de código**|Similar al diagrama de clases, muestra los objetos de código presentes dentro de un componente.|

#### Diagrama de Contexto (C1)
```mermaid
    %%{init: {
    'theme': 'base',
    'themeVariables': {
        'primaryColor': '#1168bd',
        'primaryTextColor': '#ffffff',
        'primaryBorderColor': '#0b4884',
        'lineColor': '#707070',
        'secondaryColor': '#08427b',
        'tertiaryColor': '#999999'
    }
    }}%%
    flowchart TD
        %% Título del Diagrama
        title[<b><font size=5>Diagrama de Contexto del Sistema  Nivel 1 - C4</font></b>]
        style title fill:none,stroke:none,color:#000000

        %% Personas (Usuarios) - Azul Oscuro
        admin["👤<br><b>Administrador</b><br><font size=2>[Persona]</font><br><font size=2>Usuario final que interactúa con el sistema para gestionar información.</font>"]
        repartidor["👤<br><b>Repartidor</b><br><font size=2>[Persona]</font><br><font size=2>Usuario final que interactúa con el sistema para planificar sus envíos y confirmar entregas.</font>"]
        cliente["👤<br><b>Cliente</b><br><font size=2>[Persona]</font><br><font size=2>Usuario final que interactúa con el sistema para monitorear su pedido.</font>"]

        %% Sistema Principal - Azul Claro/Rey
        sistema_principal["🚚<br><b>Sistema Trackfast</b><br><font size=2>[Sistema de Software]</font><br><font size=2>Sistema que permite gestionar el envío de paquetes</font>"]

        %% Sistemas Externos - Gris
        osrm["🗺️<br><b>Open Source Routing Machine</b><br><font size=2>[Sistema de Software Externo]</font><br><font size=2>Sistema que permite de calcular rutas en mapas</font>"]
        osm["📍<br><b>Open Source Maps</b><br><font size=2>[Sistema de Software Externo]</font><br><font size=2>Sistema abierto que proporciona datos geográficos y un mapa</font>"]

        %% Relaciones / Enlaces
        admin -->|Usa| sistema_principal
        repartidor -->|Usa| sistema_principal
        cliente -->|Usa| sistema_principal
        sistema_principal -->|Consulta datos<br><font size=2><i> JSON / REST API </i></font>| osrm
        sistema_principal -->|Consulta datos<br><font size=2><i> JSON / REST API </i></font>| osm

        %% Clases de Estilo
        classDef persona fill:#08427b,stroke:#052e56,stroke-width:1px,color:#ffffff,font-family:sans-serif;
        classDef sistema fill:#1168bd,stroke:#0b4884,stroke-width:1px,color:#ffffff,font-family:sans-serif;
        classDef externo fill:#999999,stroke:#666666,stroke-width:1px,color:#ffffff,font-family:sans-serif;

        class admin,repartidor,cliente persona;
        class sistema_principal sistema;
        class osrm,osm externo;

        %% Leyenda
        subgraph Leyenda [ ]
            direction LR
            p_leyenda[Persona]:::persona
            s_leyenda[Sistema]:::sistema
            e_leyenda[Sistema Externo]:::externo
        end
        style Leyenda fill:#fcfcfc,stroke:#cccccc,stroke-width:1px
```

#### Diagrama de Contenedores (C2)
```mermaid
    %%{init: {
  'theme': 'base',
  'themeVariables': {
    'primaryColor': '#438dd5',
    'primaryTextColor': '#ffffff',
    'primaryBorderColor': '#2e6295',
    'lineColor': '#707070',
    'secondaryColor': '#08427b',
    'tertiaryColor': '#999999',
    'fontFamily': 'sans-serif'
  }
}}%%
flowchart TD
    %% Título del Diagrama
    title["<b>Arquitectura Sistema TrackFast (Database per Service)</b>"]
    style title fill:none,stroke:none,color:#000000,font-size:18px

    %% Actores (Personas)
    repartidor["👤<br/><b>Repartidor</b><br/><small>[Persona]</small><br/><small>Usuario móvil en ruta.</small>"]
    administrador["👤<br/><b>Administrador</b><br/><small>[Persona]</small><br/><small>Gestiona la plataforma.</small>"]
    cliente["👤<br/><b>Cliente</b><br/><small>[Persona]</small><br/><small>Realiza seguimiento en vivo.</small>"]

    %% Boundary Principal del Sistema
    subgraph sys_trackfast["<b>Sistema Trackfast</b><br/><small>[Sistema de gestión de pedidos y entregas]</small>"]
        direction TB

        %% Sub-boundary: Aplicaciones Cliente
        subgraph clientes_front["<b>Aplicaciones Cliente</b>"]
            direction LR
            app_movil["📱<br/><b>App Móvil</b><br/><small>[Container: Kotlin]</small><br/><small>Gestión de paquetes, reportes de incidencias<br/>y confirmación de entrega.</small>"]
            app_web["⚛️<br/><b>Aplicación Web</b><br/><small>[Container: React.js]</small><br/><small>Plataforma administrativa y seguimiento<br/>de paquetes en vivo.</small>"]
        end

        %% API Gateway
        api_gateway["🔀<br/><b>API Gateway</b><br/><small>[Container: HTTP / JWT + JSON]</small><br/><small>Puertos: 10069, 10070.<br/>Enrutamiento y validación inicial.</small>"]

        %% Sub-boundary: Orquestación de Microservicios
        subgraph orquestacion["<b>Orquestación de Microservicios (C# / .NET)</b>"]
            direction TB

            subgraph b_auth["<b>Dominio Autenticación</b>"]
                direction TB
                ms_auth["⚙️<br/><b>MS Autenticación</b><br/><small>[Container: C# / .NET]</small><br/><small>Control de acceso, tokens y roles.</small>"]
                db_auth[("🐘<br/><b>BD Autenticación</b><br/><small>[ContainerDb: PostgreSQL]</small><br/><small>Credenciales, roles y permisos<br/>(Puerto 5432).</small>")]
            end

            subgraph b_info["<b>Dominio Gestión Información</b>"]
                direction TB
                ms_info["⚙️<br/><b>MS Gestión Información</b><br/><small>[Container: C# / .NET]</small><br/><small>Gestión de paquetes, clientes<br/>y repartidores.</small>"]
                db_info[("🐘<br/><b>BD Información</b><br/><small>[ContainerDb: PostgreSQL]</small><br/><small>Catálogo maestro de paquetes<br/>y usuarios (Puerto 5432).</small>")]
            end

            subgraph b_entregas["<b>Dominio Entregas</b>"]
                direction TB
                ms_entregas["⚙️<br/><b>MS de Entregas</b><br/><small>[Container: C# / .NET]</small><br/><small>Asignación y estado de entregas.</small>"]
                db_entregas[("🐘<br/><b>BD Entregas</b><br/><small>[ContainerDb: PostgreSQL]</small><br/><small>Historial y estados de despacho<br/>(Puerto 5432).</small>")]
            end

            subgraph b_mapa["<b>Dominio Mapa</b>"]
                direction TB
                ms_mapa["⚙️<br/><b>Microservicio Mapa</b><br/><small>[Container: C# / .NET]</small><br/><small>Recolección de ubicación y<br/>auditoría de rutas.</small>"]
                db_mapa[("🍃<br/><b>BD Mapa</b><br/><small>[ContainerDb: MongoDB]</small><br/><small>Datos geoespaciales y trazas<br/>de rutas (Puerto 27017).</small>")]
            end
        end
    end

    %% Sistemas Externos
    osrm_api["🛣️<br/><b>Open Source Route Machine</b><br/><small>[Sistema Externo]</small><br/><small>Servicio externo abierto de cálculo de rutas.</small>"]
    osm_api["🗺️<br/><b>Open Source Maps</b><br/><small>[Sistema Externo]</small><br/><small>Servicio externo abierto de datos de mapas.</small>"]

    %% Relaciones de Usuarios
    repartidor <-->|"Usa<br/><small><i>[HTTPS]</i></small>"| app_movil
    administrador <-->|"Gestiona<br/><small><i>[HTTPS]</i></small>"| app_web
    cliente <-->|"Consulta<br/><small><i>[HTTPS]</i></small>"| app_web

    %% Relaciones hacia API Gateway
    app_movil -->|"Peticiones<br/><small><i>[HTTP / JWT+JSON]</i></small>"| api_gateway
    app_web -->|"Peticiones<br/><small><i>[HTTP / JWT+JSON]</i></small>"| api_gateway

    %% Enrutamiento a Microservicios
    api_gateway -->|"Enruta<br/><small><i>[HTTP / JSON]</i></small>"| ms_auth
    api_gateway -->|"Enruta<br/><small><i>[HTTP / JSON]</i></small>"| ms_info
    api_gateway -->|"Enruta<br/><small><i>[HTTP / JSON]</i></small>"| ms_entregas
    api_gateway -->|"Enruta<br/><small><i>[HTTP / JSON]</i></small>"| ms_mapa

    %% Validación interna
    ms_auth -->|"Valida permisos<br/><small><i>[gRPC / HTTP]</i></small>"| ms_info
    ms_auth -->|"Valida permisos<br/><small><i>[gRPC / HTTP]</i></small>"| ms_entregas

    %% Persistencia aislada (Database per Service)
    ms_auth -->|"Lee/Escribe<br/><small><i>[TCP / 5432]</i></small>"| db_auth
    ms_info -->|"Lee/Escribe<br/><small><i>[TCP / 5432]</i></small>"| db_info
    ms_entregas -->|"Lee/Escribe<br/><small><i>[TCP / 5432]</i></small>"| db_entregas
    ms_mapa <-->|"Lee/Escribe<br/><small><i>[Mongo Wire / 27017]</i></small>"| db_mapa

    %% Comunicaciones Externas
    ms_mapa -->|"Genera rutas<br/><small><i>[HTTPS/JSON]</i></small>"| osrm_api
    ms_mapa -->|"Consulta datos<br/><small><i>[HTTPS/JSON]</i></small>"| osm_api

    %% Leyenda
    subgraph Leyenda["<b>Leyenda</b>"]
        direction LR
        l_persona["Persona"]
        l_container["Contenedor / BD"]
        l_externo["Sistema Externo"]
    end

    %% Estilos de Nodos (Paleta oficial C4)
    classDef persona fill:#08427b,stroke:#052e56,stroke-width:1.5px,color:#ffffff
    classDef container fill:#438dd5,stroke:#2e6295,stroke-width:1.5px,color:#ffffff
    classDef externo fill:#999999,stroke:#6b6b6b,stroke-width:1.5px,color:#ffffff

    class repartidor,administrador,cliente,l_persona persona
    class app_movil,app_web,api_gateway,ms_auth,db_auth,ms_info,db_info,ms_entregas,db_entregas,ms_mapa,db_mapa,l_container container
    class osrm_api,osm_api,l_externo externo

    %% Estilos de Boundaries (Líneas punteadas estilo C4-PlantUML)
    style sys_trackfast fill:#ffffff,stroke:#444444,stroke-width:1.5px,stroke-dasharray: 6 4,color:#000000
    style clientes_front fill:#f8fafc,stroke:#666666,stroke-width:1px,stroke-dasharray: 5 5,color:#000000
    style orquestacion fill:#f8fafc,stroke:#666666,stroke-width:1px,stroke-dasharray: 5 5,color:#000000
    style b_auth fill:#ffffff,stroke:#888888,stroke-width:1px,stroke-dasharray: 4 4,color:#000000
    style b_info fill:#ffffff,stroke:#888888,stroke-width:1px,stroke-dasharray: 4 4,color:#000000
    style b_entregas fill:#ffffff,stroke:#888888,stroke-width:1px,stroke-dasharray: 4 4,color:#000000
    style b_mapa fill:#ffffff,stroke:#888888,stroke-width:1px,stroke-dasharray: 4 4,color:#000000
    style Leyenda fill:#fcfcfc,stroke:#cccccc,stroke-width:1px,color:#000000
```