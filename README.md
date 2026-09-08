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
    subgraph 🖥️ Control Plane (Nodo Central)
        P[🔥 Prometheus Server <br/> :9090] -->|📊 Scrape /metrics cada 15s| N1
        P -->|📊 Scrape /metrics cada 15s| N2
        P -->|📊 Scrape /metrics cada 15s| N3
        P -->|📊 Scrape /metrics cada 15s| N4
        P -->|📊 Scrape /metrics cada 15s| N5
    end

    subgraph 🌐 Clúster de Servidores Ubuntu (5 Nodos)
        N1[💻 Node 01 <br/> ⚙️ Node Exporter :9100]
        N2[💻 Node 02 <br/> ⚙️ Node Exporter :9100]
        N3[💻 Node 03 <br/> ⚙️ Node Exporter :9100]
        N4[💻 Node 04 <br/> ⚙️ Node Exporter :9100]
        N5[💻 Node 05 <br/> ⚙️ Node Exporter :9100]
    end
