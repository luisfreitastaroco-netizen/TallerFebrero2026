# Obligatorio Taller de servidores linux 2026

**1. Objetivo**

    Construir una infraestructura mínima y reproducible, automatizada con Ansible y versionada en
    GitHub, integrando:

    ● NFS Server en CentOS Stream 9

    ● Cliente Ubuntu 24.04 usando automount (autofs)

    ● Un servicio systemd en Ubuntu que publique un directorio compartido usando Python
    http.server

    ● Repositorio Git con estructura clara y ejecución idempotente


  **2. Arquitectura**
  
  
    Nodos (mínimo)
  
    ● nfs01 (CentOS Stream 9): servidor NFS

    ● app01 (Ubuntu 24.04): cliente con autofs + servicio HTTP systemd
