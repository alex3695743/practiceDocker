# Configuraciones #
## Introducción ##
El proposito de este documento consiste en explicar como se han realizado las configuraciones de dos ficheros llamados docker-compose.yml que cumplen con un par de servicios cada uno de manera independiente.
## Drupal + MySQL ##
Drupal y MySQL han sido los dos servicios que ocuparan el primer archivo docker-compose.yml
Ahora veremos como están configurados cada uno y su funcionamiento
### MySQL ###
- Se utiliza la imagen oficial de MySQL en su versión 5.7
- El contenedor se nombrará explicitamente como mysql_container para facilitar su identificación
- Las variables de entorno se han configurado para que sean las necesarias para inicializar la base de datos
- Se monta el volumen mysql_data en la ruta /var/lib/mysql, donde MySQL almacena sus datos, garantizando que los datos se mantendrán aunque el contenedor se detenga o elimine.
- Asignamos el contenedor a la red personalizada drupal_network para que pueda comunicarse con otros servicios.
### Drupal ###
- Se utiliza la imagen oficial de Drupal en su última versión
- Nombramos al contenedor drupal_container
- Asignamos el puerto 81 del host que se mapea al puerto 80 del contenedor, permitiendo que Drupal pueda acceder a traves de la URL http://localhost:81
- Indicamos que depende del servicio MySQL, es decir, este debe iniciarse antes de iniciar el servicio Drupal
- Configuramos las variables de entorno para que Drupal se conecte con MySQL
- El volumen drupal_data se monta en /var/www/html, que es el directorio donde Drupal almacena su contenido y configuración.
- Asignamos el contenedor a la red drupal_network
### Volúmenes ###
- Se definen dos volúmenes, uno para guardar los datos de la base de datos para que no se pierdan tras reiniciar o eliminar el contenedor y otro para almacenar el contenido y la configuración del sitio Drupal
### Redes ###
- Se crea una red personalizada llamada drupal_network que permite que ambos contenedores se comuniquen internamente.
### Ejecución ###
Para poder ejecutarlo en la misma ruta, tendremos que ejecutar el comando "docker-compose up -d".
Para comprobar su funcionamiento nos dirigimos a la dirección 'localhost:81' dirigiendonos a la página de configuración inicial de Drupal.
## Wordpress + Mariadb ##
Wordpress y Mariadb han sido los dos servicios que ocuparan el primer archivo docker-compose.yml
Ahora veremos como están configurados cada uno y su funcionamiento
### Wordpress ###
- Utilizamos la última versión de WordPress
- Mapeamos el puerto 82 del host al puerto 80 del contenedor, por tanto WordPress será accesible mediante la utl https://localhost:82
- Configuramos las variables de entorno para que Wordpress se conecte a la base de datos
- Asignamos el contenedor a la red personalizada redDocker, permitiendo que WordPress se comunique con el contenedor de MariaDB
### MariaDB ###
- Utilizamos la imagen oficial de MariaDB en su última versión
- Establecemos las variables de entorno para la configuración inicial de la base de datos
- Asigna el contenedor a la red RedDocker, permitiendo que se comunique con el servicio de WordPress.
### Redes ###
- Se crea una red personalizada redDocker utilizando el controlador de red tipo bridge
- Esta red permite que los contenedores WordPress y MariaDB se comuniquen internamente usando sus nombres de servicio.
### Ejecución y comprobaciones ###
Para poder ejecutarlo en la misma ruta, tendremos que ejecutar el comando "docker-compose up -d".
Para comprobar su funcionamiento nos dirigimos a la dirección 'localhost:82' dirigiendonos a la página de configuración inicial de WordPress.