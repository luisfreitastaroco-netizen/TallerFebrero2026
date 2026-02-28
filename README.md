# Obligatorio Taller de servidores linux 2026

Este repositorio contiene la configuración automatizada mediante Ansible para el despliegue de una infraestructura de red funcional que integra servicios de almacenamiento compartido y servicios web sobre sistemas híbridos (CentOS y Ubuntu)

**1. Objetivo**

    Construir una infraestructura mínima y reproducible, automatizada con Ansible y versionada en
    GitHub, integrando:

    ● NFS Server en CentOS Stream 9

    ● Cliente Ubuntu 24.04 usando automount (autofs)

    ● Un servicio systemd en Ubuntu que publique un directorio compartido usando Python
    http.server

    ● Repositorio Git con estructura clara y ejecución idempotente


  **2. Arquitectura**
  
  
    Nodos (mínimo)Estructura del Repositorio

  
    ● nfs01 (CentOS Stream 9): servidor NFS

    ● app01 (Ubuntu 24.04): cliente con autofs + servicio HTTP systemd

**3. Estructura del Repositorio**

    La organización del proyecto sigue las mejores prácticas de Ansible:

    .
    ├── collections/          # Colecciones externas de Ansible
    ├── files/                # Archivos estáticos para distribución
    ├── group_vars/           # Variables de configuración por grupos
    ├── inventories/
    │   └── hosts.ini         # Definición de nodos (centos01, ubuntu01)
    ├── playbooks/
    │   ├── hardening.yaml    # Configuración de seguridad inicial
    │   ├── nfs-client.yaml   # Configuración de autofs en Ubuntu    
    │   ├── nfserver.yaml     # Configuración de exportaciones en CentOS
    │   ├── ubuntu-ufw.yaml   # Reglas de firewall para el cliente
    │   └── webserver.yaml    # Despliegue del servicio Python http.server
    ├── templates/            # Plantillas Jinja2 dinámicas
    │   ├── auto.fs           # Configuración de mapas de autofs
    │   ├── nfs_autofs        # Configuración de puntos de montaje
    │   ├── shared-http.service # Unidad de archivos de Systemd para Python
    │   └── README-NFS.j2     # Documentación dinámica para el compartido
    ├── requirements.yaml     # Dependencias de roles/colecciones
    ├── site.yaml             # Playbook maestro (ejecutable)
    ├── LICENSE
    └── README.md
 
