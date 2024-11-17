# Practica-iaw-1.6
Repositorio para la práctica 1.6. de IAW

Antonio Jesus Galvez Rodriguez

# 1 Instalación de WordPress en el directorio raíz
En esta practica vamos a explicar la instalacion de **WordPress** en el directorio raiz de Apache. Lo haremos en el archivo `deploy_wordpress_root_directory.sh` que estará en la carpeta `scripts`.

## 1.1 Eliminamos el ultimo WordPress que hemos descargado.
```
rm -rf /tmp/latest.tar.gz
```

## 1.2 Descargamos la última versión de WordPress con el comando wget.
```
wget https://wordpress.org/latest.tar.gz -P /tmp
```
El parámetro `-P` indica la ruta donde se guardará el archivo.

## 1.3 Descomprimimos el archivo .tar.gz que acabamos de descargar con el comando tar.
```
tar -xzvf /tmp/latest.tar.gz -C /tmp
```
`-x`: Indica que queremos extraer el contenido del archivo.
`-z`: Indica que queremos descomprimir el archivo.
`-v`: Habilita el modo verboso para mostrar por pantalla el proceso de descompresión.
`-f`: Se utiliza para indicar cuál es el nombre del archivo de entrada.
`-C`: Se utiliza para indicar cuál es el diretorio destino.

## 1.4 Borramos instalaciones precias que haya en /var/www/html de WordPress.
```
rm -rf /var/www/html/*
```

## 1.5 El contenido se ha descomprimido en una carpeta que se llama wordpress. Ahora, movemos el contenido de /tpm/wordpress a /var/www/html.
```
mv -f /tmp/wordpress/* /var/www/html
```

## 1.6 Creamos la base de datos y el usuario para WordPress.
```
mysql -u root <<< "DROP DATABASE IF EXISTS $WORDPRESS_DB_NAME"
mysql -u root <<< "CREATE DATABASE $WORDPRESS_DB_NAME"
mysql -u root <<< "DROP USER IF EXISTS $WORDPRESS_DB_USER@$IP_CLIENTE_MYSQL"
mysql -u root <<< "CREATE USER $WORDPRESS_DB_USER@$IP_CLIENTE_MYSQL IDENTIFIED BY '$WORDPRESS_DB_PASSWORD'"
mysql -u root <<< "GRANT ALL PRIVILEGES ON $WORDPRESS_DB_NAME.* TO $WORDPRESS_DB_USER@$IP_CLIENTE_MYSQL"
```
 Las variables `$WORDPRESS_DB_NAME`, `$WORDPRESS_DB_USER`, `$WORDPRESS_DB_PASSWORD` y `$IP_CLIENTE_MYSQL` estarán definidas en el archivo `.env`.

 ## 1.7 Creamos un archivo de configuración wp-config.php a partir del archivo de ejemplo wp-config-sample.php.
 ```
 cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
 ```

 ## 1.8 Configuramos el archivo de configuracion de WordPress.
 Para realizar este paso utilizaremos el comando `sed`.
 ```
 sed -i "s/database_name_here/$WORDPRESS_DB_NAME/" /var/www/html/wp-config.php
sed -i "s/username_here/$WORDPRESS_DB_USER/" /var/www/html/wp-config.php
sed -i "s/password_here/$WORDPRESS_DB_PASSWORD/" /var/www/html/wp-config.php
sed -i "s/localhost/$WORDPRESS_DB_HOST/" /var/www/html/wp-config.php
```

## 1.9 Cambiamos el propietario y el grupo al directorio /var/www/html.
```
chown -R www-data:www-data /var/www/html/
```

## 1.10 COnfigurar el archivo `.htaccess`.
Por último, para que todo lo anterior funcione deberemos añadir la directiva `AllowOverride All` en el archivo de configuracioón `000-default.conf` para que asi el servidor web Apache pueda leer el archivo `.htaccess`.

# 2 Instalación de WordPress en su propio directorio
En esta otra parte de la practica vamos a explicare como llevar a cabo la instalación de **WordPress** en su propio directorio mediante el archivo `deply_wordpress_own_directory.sh` en la carpeta `scripts`.

## 2.1 Eliminamos el ultimo WordPress que hemos descargado.
```
rm -rf /tmp/latest.tar.gz
```

## 2.2 Descargamos la última versión de WordPress con el comando wget.
```
wget https://wordpress.org/latest.tar.gz -P /tmp
```
El parámetro `-P` indica la ruta donde se guardará el archivo.

## 2.3 Descomprimimos el archivo .tar.gz que acabamos de descargar con el comando tar.
```
tar -xzvf /tmp/latest.tar.gz -C /tmp
```
`-x`: Indica que queremos extraer el contenido del archivo.
`-z`: Indica que queremos descomprimir el archivo.
`-v`: Habilita el modo verboso para mostrar por pantalla el proceso de descompresión.
`-f`: Se utiliza para indicar cuál es el nombre del archivo de entrada.
`-C`: Se utiliza para indicar cuál es el diretorio destino.

## 2.4 Borramos instalaciones precias que haya en /var/www/html de WordPress.
```
mkdir -p /var/www/html/$WORDPRESS_DIRECTORY
```

## 2.5 El contenido se ha descomprimido en una carpeta que se llama wordpress. Ahora, movemos el contenido de /tpm/wordpress a /var/www/html.
```
mv -f /tmp/wordpress/* /var/www/html/$WORDPRESS_DIRECTORY
```

## 2.6 Creamos la base de datos y el usuario para WordPress.
```
mysql -u root <<< "DROP DATABASE IF EXISTS $WORDPRESS_DB_NAME"
mysql -u root <<< "CREATE DATABASE $WORDPRESS_DB_NAME"
mysql -u root <<< "DROP USER IF EXISTS $WORDPRESS_DB_USER@$IP_CLIENTE_MYSQL"
mysql -u root <<< "CREATE USER $WORDPRESS_DB_USER@$IP_CLIENTE_MYSQL IDENTIFIED BY '$WORDPRESS_DB_PASSWORD'"
mysql -u root <<< "GRANT ALL PRIVILEGES ON $WORDPRESS_DB_NAME.* TO $WORDPRESS_DB_USER@$IP_CLIENTE_MYSQL"
```
Las variables `$WORDPRESS_DB_NAME`, `$WORDPRESS_DB_USER`, `$WORDPRESS_DB_PASSWORD` y `$IP_CLIENTE_MYSQL` estarán definidas en el archivo `.env`.

## 2.7 Creamos un archivo de configuración wp-config.php a partir del archivo de ejemplo wp-config-sample.php.
 ```
cp /var/www/html/$WORDPRESS_DIRECTORY/wp-config-sample.php /var/www/html/$WORDPRESS_DIRECTORY/wp-config.php

 ```

## 2.8 Configuramos el archivo de configuracion de WordPress.
En este paso tenemos que configurar las variables de configuración del archivo de configuración de WordPress. El contenido original del archivo wp-config.php será similar a este:

Para realizar este paso utilizaremos el comando `sed`.
 ```
sed -i "s/database_name_here/$WORDPRESS_DB_NAME/" /var/www/html/wp-config.php
sed -i "s/username_here/$WORDPRESS_DB_USER/" /var/www/html/wp-config.php
sed -i "s/password_here/$WORDPRESS_DB_PASSWORD/" /var/www/html/wp-config.php
sed -i "s/localhost/$WORDPRESS_DB_HOST/" /var/www/html/wp-config.php
```

Tenga en cuenta que las variables `$WORDPRESS_DB_NAME`, `$WORDPRESS_DB_USER`, `$WORDPRESS_DB_PASSWORD` y `$WORDPRESS_DB_HOST` estarán definidas en el archivo `.env`.

Si hemos realizado la instalación de **WordPress** en el directorio `miwordpress` tendremos que asignarles los siguientes valores:
```
sed -i "/DB_COLLATE/a define('WP_SITEURL', 'https://$LE_DOMAIN/$WORDPRESS_DIRECTORY');" /var/www/html/$WORDPRESS_DIRECTORY/wp-config.php
sed -i "/WP_SITEURL/a define('WP_HOME', 'https://$LE_DOMAIN');" /var/www/html/$WORDPRESS_DIRECTORY/wp-config.php
```

## 2.9 Copiamos el archivo /var/www/html/wordpress/index.php a /var/www/html.
```
cp /var/www/html/$WORDPRESS_DIRECTORY/index.php /var/www/html
```

## 2.10 Configuramos el archivo index.php
Para realizar este paso utilizaremos el comando sed.
```
sed -i "s#wp-blog-header.php#$WORDPRESS_DIRECTORY/wp-blog-header.php#" /var/www/html/index.php 
```

## 2.11 Cambiamos el propietario y el grupo al directorio /var/www/html.
```
chown -R www-data:www-data /var/www/html/
```

## 2.12 Eliminamos los valores por defecto de las security keys que aparecen en el archivo de configuración.
```
sed -i "/AUTH_KEY/d" /var/www/html/miwordpress/wp-config.php
sed -i "/SECURE_AUTH_KEY/d" /var/www/html/miwordpress/wp-config.php
sed -i "/LOGGED_IN_KEY/d" /var/www/html/miwordpress/wp-config.php
sed -i "/NONCE_KEY/d" /var/www/html/miwordpress/wp-config.php
sed -i "/AUTH_SALT/d" /var/www/html/miwordpress/wp-config.php
sed -i "/SECURE_AUTH_SALT/d" /var/www/html/miwordpress/wp-config.php
sed -i "/LOGGED_IN_SALT/d" /var/www/html/miwordpress/wp-config.php
sed -i "/NONCE_SALT/d" /var/www/html/miwordpress/wp-config.php
```

## 2.13 Hacemos una llamada a la API de wordpress para obtener las security keys y almacenamos el resultado en una variable de entorno.
```
SECURITY_KEYS=$(curl https://api.wordpress.org/secret-key/1.1/salt/)
```

## 2.14 Añadimos las security keys al archivo de configuración.
```
sed -i "/@-/a $SECURITY_KEYS" /var/www/html/miwordpress/wp-config.php
```

## 2.15 Cambiamos el propietario y el grupo al directorio /var/www/html.
```
chown -R www-data:www-data /var/www/html/
```