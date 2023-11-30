# Pila Lamp en AWS

Utilizaremos una serie de tecnologías para poder ejecutar la práctica:

- SO Debian 12
- Servidos Apache
- Php
- Basede de datos MaiaDB

## 1. Desplegamos un servidor en AWS

- Primero que nada lanzamos una instacia y le damos nombre, tambienpodemos elegir el SO que deseamos para dicho Servidor en este caso elegiré Debian 12.

![foto1](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/bbf992e3-c2db-4658-a47e-98ad5f4413f9)

- En Segundo lugar seguiremos hacia abajo y elegiremos crear un nuevo par de claves.

![foto2](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/9c5af9b5-5be0-401c-9ffc-b98e0c2e7109)

- Luego crearemos el par de claves y le daremos nombre. Dichas claves se descargán automaticamente en la carpeta de descargas.

![foto3](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/98060a92-7664-420a-80ed-2cc8cfcaa443)

- Después configuraremos la red. Elegiremos la red predeterminada o default porque no hace falta que modifiquemos los valores de la red y le damos un nuevo nombre al grupo de seguridad.

![foto4](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/12998229-1041-484c-98f0-6781e5f46982)

- Por último configuraremos los puertos, seleccionaremos el servicio HTTP para poder abrir los puertos en este caso el puerto 80. En tipo de origen eseleccionamos personalizada y en origen 0.0.0.0/0 para que pueda reconocer que cualquier IP pueda conectarse a nuestro servidor. Cuidado con no poner el puerto 22 que es el que nos permite conectarnos por ssh al servidor por lo tanto le daremos a agregar nueva regla de seguridad para configurar el puerto 80.

![foto5](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/0e87ecfd-8e2e-4683-8e37-09c5ef5ab39b)

- Habiendo configurado todo le daremos a lanzar instancia y ya se procederia a la creacion de la maquina/Servidor. Luedo le damos a ver todas las instancias y nos llevaria a ver que ya esrtaria creada.

![foto6](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/975eb20b-81c3-4231-af93-81894433ed16)

## 2. Procederemos a iniciar el Servidor

- Primero haremos click derecho sobre la instancia que hemmos creado anteriormente y le damos a Conectar. Esto nos derivaria hacia otra página para poder conectarnos por ssh través del terminal de nuestro equipo anfritión. 

![foto7](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/3d62525f-417e-45f2-aa4b-3d0a4d4aab11)

- Luego nos iremos a nuestro terminal e iremos hacia la carpeta donde tenemos las claves que nos hemos descargado anteriormente. Copiamos el comando que nos aparece (chmod...) y lo ejecutamos para cambiarle los permisos a las claves. Después copiaremos el comando (ssh -i....) que nos pone de ejemplo y lo ejecutamos, y ya estariamos dentro de nuestro servidor por ssh. 

![foto8](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/d8da0bd0-8c84-4944-aef8-550bff3580e7)

![foto9](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/153b5260-d27a-4bfd-a233-b92f1545e351)

- Ya estariamos dentro de la instancia que hemos creado.

## 3. Procederemos a instalar apache

- `# apt update`
- `# apt upgrade -y`
- `# apt install apache2 -y`
  
  Utilizaremos el siguiente comando para ver si el servicio esta activo.
  
- `# systemctl status apache2`

![foto10](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/5664b7da-0aa4-4a5d-9543-53aaf6e0dc0d)


## 4. Instalamos MariaDB

`# apt install mariadb-server`

  Para acceder a MariaDB implementaremos el siguiente comando debido a que no tenemos contrasña podemos acceder a través del root.

`# mariadb -u root -p`

1. Creamos la base de datos

`CREATE DATABASE lamp_db CHARSET utf8mb4;`

2. Procedemos a entrar en la base de datos

`USE lamp_db;`

3. Creamos la tabla

`CREATE TABLE users (
id int(11) NOT NULL auto_increment,
name varchar(100) NOT NULL,
age int(3) NOT NULL,
email varchar(100) NOT NULL,
PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;`

4. Creamos un usuario

  Podeis crear el usuario con el nombre que querais, en mi caso voy a poner mi propio nombre.

`CREATE USER IF NOT EXISTS 'julio';` 

5. Cambiamos la contraseña

`ALTER USER 'julio' IDENTIFIED BY 'julio'; `

6. Concederemos los privilegios

`GRANT ALL PRIVILEGES ON lamp_db.* TO 'julio';`

 7. Actualizaremos los privilegios

`FLUSH PRIVILEGES;` 

## 5. Intalaremos php y phpmyadmin

Ahora intalaremos php y phpmydmin en el servidor una vez fuera de la base de datos (para salir de la BD ponemos el comando quit)

- `# apt install php -y`
- `# apt install phpmyadmin -y`

   También es necesario instalar las librerias de php
  
- `# apt install libapache2-mod-php php-mysql`

1. Elegiremos las dos opciones que nos propone dandole al espacio y luego intro

![foto11](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/5e51bbfc-65d8-4a29-b9e3-56c32530fa1e)

2. Añadimos una contraseña y la confirmamos luego.

![foto12](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/a9034855-5e11-4ae7-8e73-61258f524a8c)

![foto13](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/56384a3e-2c89-49b8-ad83-2f312060d48e)

## 6. Configuraremos el archivo /var/www/html

1. Ahora descargaremos el archivo de nuetro repositorio GitHub, si no tenemos git instalado procederemos a instalarlo.

- `# apt install git`

- `git clone https://github.com/julioWestbrook/Pila-Lamp-en-AWS.git`

![foto14](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/4fa812c4-94d0-4e56-9baa-6306fac5527f)

2. Vamos hacia la carpeta src que hemos dejado en /home y ejecutamos el siguiente comando para copiar todo su contenido en /var/www/html

- `cp -r * /var/www/html`

![foto15](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/e84d01f7-29c2-4a4c-b769-266f4d9800c1)

3. Por ultimo accedemos a la web utilizando http://mi_ip_publica directamente en un navegador que la encontraremos en la instancia cuando le damos a conectar y la veremos en la pestaña conexion de la instancia ec2.

![foto16](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/3b2ad96d-2fc9-4be7-8d1b-7a75cec059af)


![foto17](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/cb6a4c9e-2238-4a3c-96c8-92a96c7538ce)



## 6. PHP my admin

Podemos añadir a la ip publica en el navegador http://mi_ip_publica/phpmyadmin accederemos a la Base de Datos que hemos creado con la contraseña y el usuario que hemos creado en MariaDB. 

![foto18](https://github.com/julioWestbrook/Pila-Lamp-en-AWS/assets/114906861/072184ea-81d4-40cd-b85e-11c5902c2e5e)





