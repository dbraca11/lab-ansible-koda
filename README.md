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


## Arquitectura y Flujo del Sistema

```mermaid
graph TD
    Start([Inicio: Infraestructura Lista]) --> Control[Nodo Controlplane - Ansible]
    Control -->|Ejecuta node_exporter.yml| ExporterTask[Despliegue Node Exporter v1.8.2]
    ExporterTask -->|Distribuye a 5 Nodos| NodesCluster[(Cluacuter Ubuntu Noble)]
    NodesCluster -->|Expone metricas| NodePorts[Puerto 9100 Activo]

    Control -->|Ejecuta prometheus.yml| PromTask[Instalacion Prometheus v2.54.1]
    PromTask -->|Configura targets| PromConfig[Archivo prometheus.yml]
    PromConfig -->|Crea servicio systemd| PromService[Servicio Prometheus Puerto 9090]

    PromService -->|Scrape HTTP cada 15s| NodePorts
    NodePorts -->|Retorna metricas de sistema| PromService

    PromService -->|Almacena TSDB| Storage[(Base de Datos TSDB)]
    Control -->|Consulta API| ApiCheck[cURL a api v1 targets]
    ApiCheck -->|Valida salud| Result{Estado de Salud?}

    Result -->|health up| Success([5 de 5 Nodos Operativos])
    Result -->|health down| Error[Alerta de Conectividad]

    style Start fill:#23272a,stroke:#fff,stroke-width:2px,color:#fff
    style Success fill:#28a745,stroke:#fff,stroke-width:2px,color:#fff
    style Error fill:#dc3545,stroke:#fff,stroke-width:2px,color:#fff
    style Control fill:#17a2b8,stroke:#fff,stroke-width:2px,color:#fff
    style PromService fill:#ffc107,stroke:#333,stroke-width:2px,color:#000
