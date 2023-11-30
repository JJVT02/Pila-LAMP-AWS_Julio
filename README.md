# Pila-LAMP-AWS_Julio
Comenzamos una instancia y le damos nombre. También podemos elegir el SO que deseamos para dicho Servidor; en este caso, elegiré Debian 12.
Captura1

En segundo lugar, seguiremos hacia abajo y elegiremos crear un nuevo par de claves.
Captura2

Luego crearemos el par de claves y le daremos nombre. Dichas claves se descargarán automáticamente en la carpeta de descargas.
Captura3

Después configuraremos la red. Elegiremos la red predeterminada o default porque no hace falta que modifiquemos los valores de la red y le damos un nuevo nombre al grupo de seguridad.
Captura4

Por último, configuraremos los puertos, seleccionaremos el servicio HTTP para poder abrir los puertos, en este caso, el puerto 80. En tipo de origen seleccionamos personalizada y en origen 0.0.0.0/0 para que pueda reconocer que cualquier IP pueda conectarse a nuestro servidor. Cuidado con no poner el puerto 22, que es el que nos permite conectarnos por SSH al servidor. Por lo tanto, le daremos a agregar nueva regla de seguridad para configurar el puerto 80.
Captura5

Habiendo configurado todo, le daremos a lanzar instancia y ya se procedería a la creación de la máquina/servidor. Luego le damos a ver todas las instancias y nos llevaría a ver que ya estaría creada.
Captura6

2. Procederemos a iniciar el Servidor
Primero haremos clic derecho sobre la instancia que hemos creado anteriormente y le damos a Conectar. Esto nos derivaría hacia otra página para poder conectarnos por SSH a través del terminal de nuestro equipo anfitrión.
Captura7

Luego nos iremos a nuestro terminal e iremos hacia la carpeta donde tenemos las claves que nos hemos descargado anteriormente. Copiamos el comando que nos aparece (chmod...) y lo ejecutamos para cambiarle los permisos a las claves. Después copiaremos el comando (ssh -i....) que nos pone de ejemplo y lo ejecutamos, y ya estaríamos dentro de nuestro servidor por SSH.
Captura8

Captura9

Ya estaríamos dentro de la instancia que hemos creado.
3. Procederemos a instalar Apache
# apt update

# apt upgrade -y

# apt install apache2 -y

Utilizaremos el siguiente comando para ver si el servicio está activo.

# systemctl status apache2

Captura10

4. Instalamos MariaDB
# apt install mariadb-server

Para acceder a MariaDB implementaremos el siguiente comando, debido a que no tenemos contraseña podemos acceder a través del root.

# mariadb -u root -p

Creamos la base de datos
CREATE DATABASE lamp_db CHARSET utf8mb4;

Procedemos a entrar en la base de datos
USE lamp_db;

Creamos la tabla
CREATE TABLE users ( id int(11) NOT NULL auto_increment, name varchar(100) NOT NULL, age int(3) NOT NULL, email varchar(100) NOT NULL, PRIMARY KEY (id) ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

Creamos un usuario
Podéis crear el usuario con el nombre que queráis, en mi caso voy a poner "julio".

CREATE USER IF NOT EXISTS 'julio';

Cambiamos la contraseña
ALTER USER 'julio' IDENTIFIED BY 'julio';

Concederemos los privilegios
GRANT ALL PRIVILEGES ON lamp_db.* TO 'julio';

Actualizaremos los privilegios
FLUSH PRIVILEGES;

5. Instalaremos PHP y phpMyAdmin
Ahora instalaremos PHP y phpMyAdmin en el servidor, una vez fuera de la base de datos (para salir de la BD ponemos el comando quit).

# apt install php -y

# apt install phpmyadmin -y

También es necesario instalar las librerías de PHP.

# apt install libapache2-mod-php php-mysql

Elegiremos las dos opciones que nos propone dándole al espacio y luego intro
![Captura11](https://github.com/LarryWestbrook/Pila-Lamp-en-AWS/assets/114
