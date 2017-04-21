Ejercicio Apache

Para instalar el servicio de apache usaremos

`
sudo apt-get install apache2
`

Una vez instalado el servicio debemos crear los hosts virtuales para cada página

`
sudo mkdir -p /var/www/gato.com/html
sudo mkdir -p /var/www/mosquito.com/html
sudo mkdir -p /var/www/escheriacholi.es/html
sudo mkdir -p /var/www/chip555.org/html
`

Concedemos permisos para usar esas carpetas a los usuarios

`
sudo chmod -R 777 /var/www
`

Y creamos las páginas web de cada host

`
echo "<img src="http://sumedico.com/wp-content/uploads/2016/06/C%C3%B3mo_saber_si_tu_gato_te_quiere.jpg"> </img>" > /var/www/gato.com/html/index.html
`

`
echo "<img src="https://lh3.googleusercontent.com/-eyP8zU-Rw6g/VV8Dw9XKB3I/AAAAAAAAHDY/O0b6wGom-dg/s640/blogger-image--394317206.jpg"> </img>" > /var/www/mosquito.com/html/index.html
`

`
echo "<img src="hhttps://lh3.googleusercontent.com/-eyP8zU-Rw6g/VV8Dw9XKB3I/AAAAAAAAHDY/O0b6wGom-dg/s640/blogger-image--394317206.jpg"> </img>" > /var/www/escheriacholi.es/html/index.html
`

`
echo "<img src="http://g04.a.alicdn.com/kf/HTB1PkchJFXXXXaSXFXXq6xXFXXXH/200PCS-free-shipping-New-NE555-NE555P-NE555N-font-b-555-b-font-font-b-Timers-b.jpg"> </img>" > /var/www/chip555.org/html/index.html
`

Debemos crear archivos de configuración para cada uno de los host virtuales

`
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/gato.com.conf
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/mosquito.com.conf
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/escheriacoli.es.conf
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/chip555.org.conf
`

Y los configuramos


gato.com.conf

`
<VirtualHost *:80>
    ServerAdmin admin@gato.com
    ServerName gato.com
    ServerAlias www.gato.com
    DocumentRoot /var/www/gato.com/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
`

mosquito.com.conf

`
<VirtualHost *:80>
    ServerAdmin admin@mosquito.com
    ServerName mosquito.com
    ServerAlias www.mosquito.com
    DocumentRoot /var/www/mosquito.com/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
`

escheriacoli.es.conf

`
<VirtualHost *:80>
    ServerAdmin admin@escheriacoli.es
    ServerName escheriacoli.es
    ServerAlias www.escheriacoli.es
    DocumentRoot /var/www/escheriacoli.es/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
<Directory /var/www/escheriacoli.es/html>
	AuthType Basic
	AuthName "ACCESO RESTRINGIDO."
	AuthUserFile /var/www/escheriacoli.es/passwords
	Require user usuario1
</Directory>
<Directory /var/www/escheriacoli.es/html>        
	Options Indexes FollowSymLinks MultiViews
	AllowOverride  none
	Order Allow,deny
	allow from all
</Directory>
`

chip555.org.conf

`
<VirtualHost *:80>
    ServerAdmin admin@chip555.org
    ServerName chip555.org
    ServerAlias www.chip555.org
    DocumentRoot /var/www/chip555.org/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
<Directory /var/www/chip555.org/html>
	AuthType Basic
	AuthName "ACCESO RESTRINGIDO."
	AuthUserFile /var/www/chip555.org/passwords
	Require valid-user
</Directory>
<Directory /var/www/chip555.org/html>        
	Options Indexes FollowSymLinks MultiViews
	AllowOverride  none
	Order Allow,deny
	allow from all
</Directory>
`

Los hosts escheriacoli.es y chip555.org necesitan de un archivo password:

`
sudo htpasswd -c /var/www/chip555.org/passwords user01
sudo htpasswd -c /var/www/escheriacoli.es/passwords
`

Para finalizar reiniciamos el servicio de apache:

`
sudo service apache2 restart
`
