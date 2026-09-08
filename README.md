# Lab Ansible: Stack de Observabilidad Automatizado

Infraestructura como código utilizando Ansible para desplegar un stack de monitoreo y recolección de métricas en un clúster de 5 nodos.

## Arquitectura del Stack
- **Orquestador**: Ansible (`controlplane`)
- **Agente de Métricas**: Prometheus Node Exporter v1.8.2 (Puerto `9100` en cada nodo)
- **Colector Central**: Prometheus v2.54.1 (Puerto `9090` en el nodo de control)

## Estructura del Proyecto
- `inventory.ini`: Inventario con los 5 nodos del clúster y sus IPs estáticas.
- `node_exporter.yml`: Playbook de Ansible para despliegue automatizado del agente en los servidores.
- `prometheus.yml`: Playbook de Ansible para la instalación y configuración del servidor central de Prometheus.

## Instrucciones de Despliegue

1. **Instalar Node Exporter en el clúster**:
   ```bash
   ansible-playbook -i inventory.ini node_exporter.yml
Desplegar Prometheus en el control plane:

Bash
ansible-playbook -i inventory.ini prometheus.yml
Verificar Estado de los Targets:

Bash
curl -s http://localhost:9090/api/v1/targets | grep -o '"health":"[^"]*"'


graph TD
    %% Inicio del Proceso
    Start([🚀 Inicio: Infraestructura Base Lista]) --> Control[💻 Nodo Controlplane <br/> Orquestador Ansible]

    %% Fase 1: Despliegue de Agentes
    Control -->|1️⃣ Ejecuta node_exporter.yml| ExporterTask[⚙️ Despliegue Node Exporter v1.8.2]
    ExporterTask -->|Distribuye a 5 Nodos| NodesCluster[(🌐 Clúster Ubuntu Noble <br/> node01 - node05)]
    NodesCluster -->|Abre puerto y expone métricas| NodePorts[🔌 Puerto 9100 /metrics Activo]

    %% Fase 2: Configuración del Colector Central
    Control -->|2️⃣ Ejecuta prometheus.yml| PromTask[🔥 Instalación Prometheus v2.54.1]
    PromTask -->|Configura targets estáticos| PromConfig[📝 Archivo prometheus.yml <br/> Intervalo de scrape: 15s]
    PromConfig -->|Crea servicio systemd| PromService[🟢 Servicio Prometheus Iniciado <br/> Puerto 9090]

    %% Fase 3: Ciclo de Scrape y Monitoreo
    PromService -->|3️⃣ Petición HTTP GET cada 15s| NodePorts
    NodePorts -->|Retorna métricas del sistema <br/> CPU, Memoria, Disco, Red| PromService

    %% Fase 4: Verificación y Salud
    PromService -->|4️⃣ Almacena en TSDB local| Storage[(💾 Base de Datos TSDB <br/> /var/lib/prometheus)]
    Control -->|5️⃣ Consulta API de Salud| ApiCheck[🔍 cURL a /api/v1/targets]
    ApiCheck -->|Valida estado de nodos| Result{¿Estado de Salud?}

    %% Resultados
    Result -->|health: up| Success([✅ 5/5 Nodos Operativos y Monitoreados])
    Result -->|health: down| Error[⚠️ Alerta de Conectividad / Revisar Red o Servicio]

    %% Estilos
    style Start fill:#23272a,stroke:#fff,stroke-width:2px,color:#fff
    style Success fill:#28a745,stroke:#fff,stroke-width:2px,color:#fff
    style Error fill:#dc3545,stroke:#fff,stroke-width:2px,color:#fff
    style Control fill:#17a2b8,stroke:#fff,stroke-width:2px,color:#fff
    style PromService fill:#ffc107,stroke:#333,stroke-width:2px,color:#000
