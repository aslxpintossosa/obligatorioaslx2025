***********************************************************
*******README.MD OBLIGATORIO TALLER ASLX 2025********
***********************************************************
INTRODUCCION
En este archivo se encuentran instrucciones e información correspondiente a la ejecución del obligatorio 2025 del Taller de Administrador de Servidores LINUX, carrera Analista en Infraestructura Informática, Universidad ORT.
***************************************************************
**********CONFIGURACION LLAVES SSH************************
***************************************************************
Para utilizar ansible, desde la maquina bastion, primero deben configurarse las llaves para el acceso SSH, a continuación un ejemplo para CentOS 9 Stream:

Creación de la llave

ssh-keygen

elegir opción por defecto
colocarle un password

ssh-copy-id {IP}
Este comando debe repetirse para cada una de las maquinas a administrar

Una vez copiadas, probar ingresar por SSH a las maquinas a administrar

**************************************************************
***********PAQUETES NECESARIOS A INSTALAR****************
**************************************************************
UPGRADE A BASTION si es necesario para tener la última versión disponible del sistema
sudo dnf upgrade

GIT – INSTALACION, ej CentOs 9
dnf install git (version git-core-2.47.3-1.el9.x86_64)

ANSIBLE - INSTALACION, ej CentOs 9
dnf install ansible-core (version 1:2.14.18-1rl9)
---

**********************************************************
*******CLONACION DE REPOSITORIO obligatorioaslx2025*******
**********************************************************
Configuramos GIT en la maquina BASTION el usuario y el mail para que nuestras acciones e GITHUB sean registradas correctamente.
git config --global user.name "NombreDeUsuario"
git config --global usar.mail "CorreoDelUsuario"
Se debe clonar en la maquina bastion el repsoitorio a utilizar, en este caso se puede clonar mediante HTTPS URL:
para ello utilizaremos comando

git clone https://github.com/aslxpintossosa/obligatorioaslx2025.git

(Puede ser posible También descargar el zip desde el repositorio o clonarlo a su propia cuenta de GitHub)

(La creación de la estructura del repositorio, esta detallada en el archivo de documentación PDF en este mismo repositorio)

NOTA - Se debe de satisfacer los requerimientos ANTES de ejecutar los playbooks, ya que no todos los módulos se encuentran en ansible-core
el repositorio cuenta ya con la colección requerimientos predefinidos, pero debe ejecutarse antes el siguiente comando para instalar los módulos adicionales
Esto  detallara en la sección de clonación

Módulos adicionales instalados utilizando el archivo incluido

ansible.posix   (Version 1.5.4)
community.general   (Version 10.7.2)

Cambiar al directorio del repositorio (obligatorioaslx2025)

cd obligatorioaslx2025

++++++++++++++++++++++++++++++++++++++++++++
ansible-galaxy install -r collections/requirements.yaml
+++++++++++++++++++++++++++++++++++++++++++

**********************************************************
*********MODIFICACION DE VARIABLES DE INVENTARIO**********
**********************************************************
El archivo inventario se encuentra en la carpeta /inventories/inventory.ini
De ser necesario, se debe de cambiar las IP y/o usuarios para el manejo de los hosts por los del sistema a utilizar.
Por defecto se incluyen IP y el usuario sysadmin en el archivo

Para comprobar la conectividad utilizar (CentOS)

ansible all -i inventories/inventory.ini -m ping

***********************************************************
*********Posibilidad de ejecución de comandos AD-HOC*******
***********************************************************
En caso de ejecutar comandos add-hoc, esto es posible, se adjuntan 3 ejemplos
1 - Listar usuarios de servidor ubuntu01
ansible ubuntu01 -i inventories/inventory.ini -m shell -a "cut -d: -f1 /etc/passwd"
2 – Mostrar el uso de memoria en todos los servidores
ansible linux -i inventories/inventory.ini -m shell -a "free -h"
3 – Verificar que Chrony este instalado y funcionando en servidores centos
ansible centos01 -i inventories/inventory.ini -m shell -a "systemctl status chronyd"

************************************************************
******Ejecución de Tareas Automatizadas ANSIBLE*************
************************************************************
NOTA: Durante la ejecución de los playbooks, si se utilizan las versiones especificadas en este archivo, se pueden llegar a observar algunas advertencias (Warnings) sobre la compatibilidad entre estas, esto no afecta en nada la ejecución de estos playbooks, pero de querer resolverlo, se pude optar por agregar la versión para que el warning desaparezca o bien revisar la documentación de versión de las colecciones extra.

Para ejecutar los playbooks utilizando los comandos mostrados a continuación, usuario debe estar parado en el directorio raíz del repositorio en la consola del bastion
~/obligaorio2025/
ansible-playbook -i inventories/inventory.ini playbooks/nfs_setup.yaml -K

IMPORTANTE: Antes de ejecutar el siguiente playbook, asegurarse que clave publica haya sido copiada para mantener access al server (ssh-copy-id {IP})

ansible-playbook -i inventories/inventory.ini playbooks/hardening.yaml -K

___________________            _-_
\__(==========/_=_/   ____.---'---`---._____
            \_ \      \----.__ANSIBLE_.----/
              \ \     /  /    `-_-'
          __,--`.`---'..'-_
         /____  USS-ASLX ||
              `--.____,-'
