# Obligatorio Taller de servidores linux 2026

Este repositorio contiene la configuración automatizada mediante Ansible para el despliegue de una infraestructura
de red funcional que integra servicios de almacenamiento compartido y servicios web sobre sistemas híbridos (CentOS y Ubuntu)

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

 
**4.Topología (IPs / Hostnames)**

    La infraestructura se basa en una red segmentada por roles, 
    utilizando IPs estáticas para asegurar la persistencia de los montajes NFS.

    Hostname	IP	S.O.	        Rol / Grupo Ansible

    nfs01	    192.168.10.11	    CentOS Stream 9	fileserver (Servidor NFS)
    ubuntu01	192.168.10.21	    Ubuntu 24.04	appserver (Cliente + HTTP)
    ubuntu02	192.168.10.22	    Ubuntu 24.04	appserver (Nodo secundario)


**5. Requisitos Previos**

    Para asegurar una ejecución exitosa del proyecto, verifica el cumplimiento de los siguientes puntos:
    En la Máquina de Control

    Ansible v2.15 o superior: Instalado y funcional.

        ansible --version

        

    Colecciones de Ansible: Si tu proyecto usa módulos específicos (como community.general), asegúrate de instalarlas:

        ansible-galaxy collection install -r requirements.yaml

    
    Resolución de Nombres: Los hostnames nfs01 y app01 deben estar definidos en tu archivo /etc/hosts 
    o ser resolubles por DNS para que coincidan con el
    
    inventory/hosts.ini.

    
    
    En los Nodos de Destino (nfs01 y app01)

    Acceso SSH sin contraseña: Tu llave pública debe estar en el archivo ~/.ssh/authorized_keys de ambos nodos.

    
    
    Sistemas Operativos: * nfs01: CentOS Stream 9 (basado en RHEL).


    Prueba de Conectividad y Privilegios

    Antes de iniciar el despliegue principal, se recomienda ejecutar este comando para garantizar que el nodo de control
    tiene acceso SSH y capacidad de scalamiento de privilegios (Sudo) en todos los nodos administrados: Bash

    
    Verificar que todos los nodos responden y tienen sudo listo
        ansible all -i inventories/hosts.ini -m ping
        ansible all -i inventories/hosts.ini -a "sudo uptime"

    Esta prueba confirma que:

        Los hostnames/IPs en el inventario son correctos.

        Las llaves SSH están bien configuradas.

        El usuario tiene permisos para realizar las tareas de administración necesarias para NFS y Systemd.
                           app01: Ubuntu 24.04 LTS (Noble Numbat).

    
    
    Privilegios de Superusuario: El usuario definido en el inventario debe tener permisos de sudo configurados
    (preferentemente NOPASSWD) para permitir la ejecución de tareas administrativas.

    Conectividad de Red: Los nodos deben tener comunicación entre sí (ping) y salida a internet
    para la instalación de paquetes (nfs-utils, autofs, python3).



**5. Ejecucion de la solucion**

    ### Ejecutar el playbook principal

    ---bash
    # Ejecutar todos los playbooks
        ansible-playbook -i inventories/hosts.ini site.yaml

    # Ejecutar un playbook específico
        ansible-playbook -i inventories/hosts.ini playbooks/webserver.yaml

    # Modo de prueba
        ansible-playbook -i inventories/hosts.ini site.yaml --check

    # Modo verbose para debugging
        ansible-playbook -i inventories/hosts.ini site.yaml -vvv ---

    ### Ejecutar playbooks individuales

    ---bash
    # Solo configurar firewall
        ansible-playbook -i inventories/hosts.ini playbooks/ubuntu-ufw.yaml

    # Solo configurar NFS server
        ansible-playbook -i inventories/hosts.ini playbooks/nfserver.yaml

    # Solo hardening de seguridad
        ansible-playbook -i inventories/hosts.ini playbooks/hardening.yaml
