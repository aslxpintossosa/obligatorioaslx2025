# obligatorioaslx2025
Repositorio para la realización del trabajo obligatorio de Taller ASLX 2025 Pintos-Sosa
*****************************************************
*******README.MD OBLIGATORIO TALLER ASLX 2025********
*****************************************************

INTRODUCCION

bla bla

CONFIGURACION DE REDES EN HOSTS

Para el correcto funcionamiento de las tareas en ANSIBLE a realizar, se deben primero configurar las redes de las mismas en la plataforma que sera utilizada para clonar el repositorio de GITHUB, esto es, que entre ellas haya visibilidad.

CONFIGURACION LLAVES SSH

Para poder clonar el repositorio GIT, debemos de crear las llaves publicas en nuestra maquina bastion, en el caso que este repositorio sea clonado, este paso no sera necesario.

Tambien es necesario crear las llaves de SSH entre el bastion a utilizar y las maquinas hosts.

para ello se debe utilizar:

ssh-copy-id {IP}

*********************************************************
***********PAQUETES NECESARIOS A INSTALAR****************
*********************************************************
UPGRADE A BASTION

dnf upgrade

GIT

dnf install git   (version git-core-2.47.3-1.el9.x86_64)

ANSIBLE

dnf install ansible core (version 1:2.14.18-1rl9)

**********************************************************
*******CLONACION DE REPOSITORIO obligatorioaslx2025*******
**********************************************************

Configuramos GIT en la maquina BASTION el usuario y el mail para que nuestras acciones e GITHUB sean registradas rrecametne.

git config --global user.name "aslxpintossosa"
git config --global usar.mail "aslx.pintos.sosa@gmail.com"

Se debe proceder a clonar en la maquina bastion el repsoitorio a utilizar, en este caso se puede clonar mediante SSH:

git@github.com:aslxpintossosa/obligatorioaslx2025.git

para ello utilizaremos comando

git clone git@github.com:aslxpintossosa/obligatorioaslx2025.git

(La creacion de la estructura del repositorio, esta detallada en l archivo de documentacion PDF)

***********************************************************
*********Posibilidad de ejecucion de comandos AD-HOC*******
***********************************************************
En caso de ejecutar comandos add-hoc, esto es posible, se adjuntan 3 ejemplos

1 - Listar usuarios de servidor ubuntu01
ansible ubuntu01 -i inventories/inventory.ini -m shell -a "cut -d: -f1 /etc/passwd"


