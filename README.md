# 🤖 Lab Ansible Koda

### Infraestructura como Código (IaC) con Ansible bajo estándares de producción

[![Ansible](https://img.shields.io/badge/Ansible-2.15%2B-EE0000?style=for-the-badge&logo=ansible&logoColor=white)](https://www.ansible.com/)
[![Python](https://img.shields.io/badge/Python-3.12%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)](.github/workflows/lint.yml)
[![Lint](https://img.shields.io/badge/Lint-ansible--lint-000000?style=for-the-badge&logo=ansible&logoColor=white)](.ansible-lint)
[![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

**Repositorio automatizado de infraestructura como código (IaC) para la gestión, configuración y despliegue de servidores usando Ansible bajo estrictos estándares de calidad y validación continua en CI/CD.**

[📊 Ver diagrama de arquitectura](https://dbraca11.github.io/lab-ansible-koda/diagram.html) · [🐛 Reportar issue](https://github.com/dbraca11/lab-ansible-koda/issues) · [💡 Sugerir mejora](https://github.com/dbraca11/lab-ansible-koda/issues)

</div>

---

## 📖 Tabla de contenidos

- [🎯 Descripción](#-descripción)
- [🎨 Diagrama de arquitectura](#-diagrama-de-arquitectura)
- [📁 Estructura del proyecto](#-estructura-del-proyecto)
- [🚀 Requisitos previos](#-requisitos-previos)
- [🛠️ Ejecución de playbooks](#️-ejecución-de-playbooks)
- [🔍 Validación de código (Linting)](#-validación-de-código-linting)
- [🔄 Pipeline CI/CD](#-pipeline-cicd)
- [🧩 Roles incluidos](#-roles-incluidos)
- [📚 Aprendizajes y buenas prácticas](#-aprendizajes-y-buenas-prácticas)
- [📄 Licencia](#-licencia)

---

## 🎯 Descripción

Este repositorio implementa una **infraestructura automatizada y reproducible** usando **Ansible** como motor de automatización. Todos los playbooks siguen las **buenas prácticas oficiales de Ansible** y se validan automáticamente mediante un pipeline de CI/CD en cada cambio, garantizando **idempotencia, seguridad y calidad** en cada despliegue.

### ✨ Características principales

| Característica | Descripción |
|----------------|-------------|
| 🧩 **Roles modulares** | Cada componente (firewall, nginx, users, grafana) está encapsulado en un rol reutilizable |
| 📋 **Inventarios estructurados** | Separación de entornos en `inventories/` para escalar a múltiples ambientes |
| ✅ **Linting estricto** | Perfil de producción de `ansible-lint` con reglas de idempotencia y seguridad |
| 🔄 **CI/CD automatizado** | GitHub Actions valida cada push/PR antes del merge a `main` |
| 🔒 **Gestión segura** | Configuración de usuarios, claves SSH y firewall (UFW) siguiendo estándares |

---

## 🎨 Diagrama de arquitectura

> 🖼️ **Visualización interactiva:** [Ver diagrama completo](https://dbraca11.github.io/lab-ansible-koda/diagram.html)
┌─────────────────────┐
│ 🖥️ Control Node │ Ansible Core 2.15+
└──────────┬──────────┘
│
▼
┌─────────────────────┐ ┌──────────────────┐
│ 📋 Inventario │─────▶│ 📖 Playbooks │
│ hosts.yml │ │ site.yml │
└─────────────────────┘ └────────┬─────────┘
│
▼
┌─────────────────┐
│ 🖧 Managed │
│ Servidores Linux│
│ vía SSH │
└─────────────────┘

text

---

## 📁 Estructura del proyecto
lab-ansible-koda/
├── .github/
│ └── workflows/
│ └── lint.yml # Pipeline de validación continua
├── inventories/
│ └── lab/
│ └── hosts.yml # Inventario de servidores
├── roles/
│ ├── firewall/ # Configuración de UFW
│ ├── nginx/ # Despliegue de Nginx
│ └── users/ # Gestión de usuarios y SSH
├── deploy-nginx.yml # Playbook de despliegue Nginx
├── disable-unattended-upgrades.yml # Playbook desactivación upgrades
├── grafana.yml # Playbook instalación Grafana
├── .ansible-lint # Configuración del linter
└── README.md

text

---

## 🚀 Requisitos previos

Antes de ejecutar los playbooks, asegúrate de tener instalado:

| Requisito | Versión mínima | Instalación |
|-----------|----------------|-------------|
| **Ansible Core** | 2.15+ | [Guía oficial](https://docs.ansible.com/ansible/latest/installation_guide/) |
| **Python** | 3.12+ | [python.org](https://www.python.org/downloads/) |
| **Colección community.general** | Última | Ver comando abajo |

Instala la colección requerida:

```bash
ansible-galaxy collection install community.general
🛠️ Ejecución de playbooks
Todos los comandos usan el inventario del laboratorio (inventories/lab/hosts.yml).

🌐 Desplegar y configurar Nginx
bash
ansible-playbook -i inventories/lab/hosts.yml deploy-nginx.yml
🔒 Desactivar actualizaciones automáticas
bash
ansible-playbook -i inventories/lab/hosts.yml disable-unattended-upgrades.yml
📊 Instalar y configurar Grafana
bash
ansible-playbook -i inventories/lab/hosts.yml grafana.yml
💡 Tip: Añade --check para simular los cambios sin aplicarlos, o -v/-vv para mayor detalle de la salida.

🔍 Validación de código (Linting)
El proyecto usa un perfil estricto de producción con ansible-lint. Verifica localmente antes de cada push:

bash
ansible-lint
El archivo .ansible-lint define las reglas y exclusiones aplicadas.

🔄 Pipeline CI/CD
Cada push o pull request a la rama main dispara automáticamente un workflow de GitHub Actions que:

🧪 Instala Ansible y las dependencias necesarias

🔍 Ejecuta ansible-lint con el perfil de producción

✅ Bloquea el merge si hay errores de sintaxis, idempotencia o seguridad

Ubicación: .github/workflows/lint.yml

🧩 Roles incluidos
<details> <summary><strong>🔥 firewall</strong> — Configuración de UFW y reglas de red</summary>
Define políticas por defecto (deny incoming, allow outgoing)

Habilita puertos específicos (SSH, HTTP, HTTPS)

Activa el firewall de forma segura sin bloqueos

</details><details> <summary><strong>🌐 nginx</strong> — Despliegue y configuración del servidor web</summary>
Instalación de Nginx

Configuración de virtual hosts

Habilitación de sitios y recarga controlada del servicio

</details><details> <summary><strong>👥 users</strong> — Gestión segura de usuarios y claves SSH</summary>
Creación de usuarios con grupos definidos

Configuración de claves SSH autorizadas

Gestión de sudoers sin intervención manual

</details><details> <summary><strong>📊 grafana</strong> — Instalación y configuración de Grafana</summary>
Añadir el repositorio oficial de Grafana

Instalación y configuración del servicio

Verificación del estado del servidor

</details>
📚 Aprendizajes y buenas prácticas
✅ Idempotencia: cada playbook puede ejecutarse múltiples veces sin efectos secundarios

✅ Separación por roles: cada responsabilidad vive en su propio rol reutilizable

✅ Inventarios declarativos: la infraestructura se describe, no se improvisa

✅ Validación continua: nada llega a main sin pasar el linter

✅ Versionado de colecciones: dependencias declaradas y reproducibles

📄 Licencia
Este proyecto está bajo la Licencia MIT. Consulta el archivo LICENSE para más detalles.
