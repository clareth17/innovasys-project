# Configuración del Servidor InnovaSys con Ansible

Este proyecto automatiza la configuración de un servidor Ubuntu 24.04 para que funcione como un portal web interno y un recurso compartido de archivos centralizado Samba para la startup ficticia "InnovaSys". Utiliza Ansible para el aprovisionamiento y la gestión de la configuración.

## Requisitos

*   **Nodo de Control:** Una máquina Linux (por ejemplo, Linux Lite) con Ansible instalado.
*   **Nodo Gestionado:** Una máquina servidora Ubuntu 24.04 objetivo (llamada `servidor-innovasys` en esta configuración).
*   Acceso SSH configurado entre el Nodo de Control y el Nodo Gestionado.
*   Privilegios de sudo en el Nodo Gestionado.

## Estructura del Proyecto

*   `main.yml`: El playbook principal de Ansible que invoca los roles.
*   `inventory`: Define el nodo gestionado (`servidor-innovasys`) y los detalles de conexión.
*   `group_vars/all.yml`: Contiene las variables utilizadas en los playbooks (nombre de la empresa, usuario, contraseña, rutas).
*   `roles/apache/`: Rol para instalar y configurar el servidor web Apache.
    *   `tasks/main.yml`: Tareas para la instalación, configuración y gestión del servicio Apache.
    *   `templates/index.html.j2`: Plantilla Jinja2 para la página de bienvenida personalizada.
    *   `handlers/main.yml`: Handler para reiniciar Apache cuando sea necesario.
*   `roles/samba/`: Rol para instalar y configurar el servidor de archivos Samba.
    *   `tasks/main.yml`: Tareas para la instalación de Samba, creación de usuarios/grupos, configuración del recurso compartido y gestión del servicio.
    *   `templates/smb.conf.j2`: Plantilla Jinja2 para el archivo de configuración de Samba.
    *   `handlers/main.yml`: Handler para reiniciar Samba cuando sea necesario.

## Cómo Ejecutar

1.  Asegúrese de que su archivo de inventario (`inventory`) esté configurado correctamente con la dirección IP y el usuario para su servidor Ubuntu objetivo (`servidor-innovasys`).
2.  Asegúrese de tener conectividad SSH y sudo sin contraseña (o `ansible_become_pass` configurado en el inventario) en el servidor objetivo.
3.  Navegue al directorio del proyecto en su nodo de control Ansible.
4.  Ejecute el playbook:
    ```bash
    ansible-playbook -i inventory main.yml
    ```
5.  Tras una ejecución exitosa:
    *   Acceda al portal web en `http://<IP_DEL_SERVIDOR>`.
    *   Acceda al recurso compartido Samba en `smb://<IP_DEL_SERVIDOR>/Proyectos` usando las credenciales definidas en `group_vars/all.yml`.
