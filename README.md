# Lab Ansible Koda

Repositorio automatizado de infraestructura como código (IaC) para la gestión, configuración y despliegue de servidores utilizando **Ansible** bajo estrictos estándares de calidad y validación continua en CI/CD.

## 📁 Estructura del Proyecto

```text
.
├── .github/
│   └── workflows/
│       └── lint.yml          # Pipeline de validación continua con Ansible Lint
├── inventories/
│   └── lab/
│       └── hosts.yml         # Inventario estructurado de servidores del laboratorio
├── roles/
│   ├── firewall/             # Configuración de UFW y reglas de red
│   ├── nginx/                # Despliegue y configuración del servidor web Nginx
│   └── users/                # Gestión segura de usuarios y claves SSH
├── deploy-nginx.yml          # Playbook para despliegue de Nginx
├── disable-unattended-upgrades.yml # Playbook para desactivación limpia de actualizaciones desatendidas
├── grafana.yml               # Playbook para instalación y configuración de Grafana
├── .ansible-lint             # Configuración y exclusiones para el linter
└── README.md                 # Documentación del proyecto
🚀 Requisitos Previos
Ansible Core (versión 2.15 o superior)

Python 3.12+ (recomendado para ejecución local y linter)

Colecciones necesarias instaladas:

Bash
ansible-galaxy collection install community.general
🛠️ Ejecución de Playbooks
Para ejecutar los despliegues sobre el inventario del laboratorio, utiliza los siguientes comandos:

Desplegar y configurar Nginx:

Bash
ansible-playbook -i inventories/lab/hosts.yml deploy-nginx.yml
Desactivar actualizaciones automáticas:

Bash
ansible-playbook -i inventories/lab/hosts.yml disable-unattended-upgrades.yml
Instalar Grafana:

Bash
ansible-playbook -i inventories/lab/hosts.yml grafana.yml
🔍 Validación de Código (Linting)
El proyecto cuenta con un perfil estricto de producción en ansible-lint. Para verificar localmente que todo el código cumpla con las normas de sintaxis, idempotencia y seguridad:

Bash
ansible-lint
🔄 Pipeline CI/CD
Las validaciones automáticas se ejecutan mediante GitHub Actions en cada push o pull_request dirigido a la rama principal (main), asegurando que ningún cambio que rompa el estándar de producción sea integrado.
