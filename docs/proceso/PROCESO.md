# Proceso de Trabajo Scrum

```mermaid
sequenceDiagram
    autonumber
    participant P1 as Planificar el Sprint
    participant P2 as Desarrollar en Ramas
    participant P3 as Revisar e Integrar
    participant P4 as Revisar el Sprint
    participant P5 as Retrospectiva
    participant P6 as Fin del Ciclo

    Note over P1: Entrada Inicial
    Note right of P1: Entrada:<br/>Product Backlog priorizado

    rect rgb(240, 248, 255)
    Note over P1,P2: 1. Planificar el Sprint
    P1->>P2: Salida: Objetivos Definidos
    P1->>P2: Salida: Issue Asignado
    Note right of P1: **Responsable:**<br/>Líder de proyecto<br/>* Mateo Medranda<br/>Diseñador<br/>* Gabriel López
    end

    rect rgb(245, 245, 245)
    Note over P2,P3: 2. Desarrollar en Ramas
    P2->>P3: Salida: Generación Rama con commit
    P2->>P3: Salida: Pull Request
    Note right of P2: **Responsable:**<br/>Equipo de Desarrollo<br/>DevOps<br/>* Marcelo Pareja
    end

    rect rgb(240, 248, 255)
    Note over P3,P4: 3. Revisar e Integrar
    Note right of P3: Caso Aceptación
    P3->>P4: Salida: Issue Completado
    P3->>P4: Salida: Aprobación Pull Request
    
    Note right of P3: Caso Denegación
    P4->>P3: Entrada: Denegación Pull Request
    P3->>P2: Entrada: Corrección en rama
    Note right of P3: **Responsable:**<br/>Líder de Proyecto<br/>* Mateo Medranda
    end

    rect rgb(245, 245, 245)
    Note over P4,P5: 4. Revisar el Sprint
    P4->>P5: Salida: Incremento aceptado
    P4->>P5: Salida: Aceptación/Cambios planteados
    Note right of P4: **Responsable:**<br/>Gestor de Proyecto<br/>* Mateo Medranda<br/>Cliente + Product Owner<br/>* Nancy Betancourt
    end

    rect rgb(240, 248, 255)
    Note over P5,P6: 5. Retrospectiva
    P5->>P6: Salida final: Lecciones aprendidas
    P5->>P6: Salida final: Plan de mejora
    Note right of P5: **Responsable:**<br/>Equipo de Desarrollo
    end

    Note over P1: Ciclo continuo
    Note right of P1: Nueva entrada:<br/>Mejoras + Backlog actualizado