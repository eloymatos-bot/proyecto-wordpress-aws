# 🚀 Despliegue de WordPress en AWS EC2

## 📌 Descripción

En este proyecto se implementó un servidor web en la nube utilizando Amazon Web Services (AWS EC2), configurando un entorno completo con Apache, MySQL y WordPress.
El objetivo fue desplegar un sitio web funcional, aplicar configuraciones básicas de seguridad y documentar todo el proceso técnico.

---

## 🛠️ Tecnologías utilizadas

* AWS EC2
* Ubuntu Server
* Apache
* MySQL
* WordPress
* UFW (Firewall)

---

## ⚙️ Implementación paso a paso

### 🔹 1. Creación de la instancia EC2

Se creó una instancia en AWS EC2 utilizando Ubuntu Server como sistema operativo y el tipo t2.micro.
Durante la configuración se habilitaron los puertos necesarios:

* 22 (SSH)
* 80 (HTTP)
* 443 (HTTPS)

Esto permitió la conexión remota al servidor y el acceso web desde el navegador.

---

### 🔹 2. Conexión al servidor mediante SSH

Se accedió al servidor utilizando una clave `.pem` desde la terminal:

```bash
ssh -i clave.pem ubuntu@IP_PUBLICA
```

Esto permitió administrar el servidor de forma remota.

---

### 🔹 3. Instalación de Apache

Se actualizó el sistema y se instaló Apache como servidor web:

```bash
sudo apt update
sudo apt install apache2
```

Luego se verificó su funcionamiento accediendo a la IP pública desde el navegador.

---

### 🔹 4. Instalación y configuración de MySQL

Se instaló MySQL Server:

```bash
sudo apt install mysql-server
sudo mysql_secure_installation
```

Se configuró la base de datos para WordPress:

```sql
CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'password123';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
```

Esto permitió almacenar la información del sitio web.

---

### 🔹 5. Instalación de WordPress

Se descargó WordPress desde su sitio oficial y se configuró en el servidor:

```bash
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
sudo mv wordpress /var/www/html/
```

Se asignaron permisos adecuados para su funcionamiento.

---

### 🔹 6. Configuración de Apache (VirtualHost)

Se creó un archivo de configuración para el sitio:

```bash
sudo nano /etc/apache2/sites-available/wordpress.conf
```

Luego se activó el sitio y se reinició Apache:

```bash
sudo a2ensite wordpress.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```

#### ⚠️ Problema encontrado

Inicialmente Apache mostraba la página por defecto.
Esto se solucionó desactivando el sitio por defecto:

```bash
sudo a2dissite 000-default.conf
sudo systemctl restart apache2
```

---

### 🔹 7. Configuración de WordPress

Se accedió desde el navegador a través de la IP pública y se completó la instalación:

* Base de datos: wordpress
* Usuario: wpuser
* Contraseña: password123

Se logró acceder correctamente al panel de administración.

---

### 🔹 8. Creación de usuario Linux

Se creó un usuario personalizado para mejorar la seguridad:

```bash
sudo adduser eloy
```

Luego se le asignaron permisos de administrador:

```bash
sudo usermod -aG sudo eloy
```

Se cambió al nuevo usuario:

```bash
su - eloy
```

#### ⚠️ Problema encontrado

Durante este proceso apareció el error:

> No password has been supplied

Esto ocurrió porque no se ingresó correctamente la contraseña.
Se solucionó repitiendo el proceso e ingresando una contraseña válida.

---

### 🔹 9. Configuración del firewall (UFW)

Se configuró el firewall para permitir solo conexiones necesarias:

```bash
sudo ufw allow OpenSSH
sudo ufw allow 'Apache Full'
sudo ufw enable
sudo ufw status
```

Esto permitió asegurar el servidor contra accesos no autorizados.

---

## 🌐 Desarrollo del sitio web

Se desarrolló un sitio web básico en WordPress con las siguientes secciones:

* Inicio
* Nosotros
* Productos

Se configuró un menú de navegación utilizando el editor moderno de WordPress.

#### ⚠️ Problemas encontrados

* No se encontraba la opción “Apariencia → Menús” debido a la nueva interfaz de WordPress
* Se tuvo dificultad para ubicar la opción de navegación
* Se solucionó utilizando el Editor del sitio y la sección “Navegación”

---

## ✅ Verificación

Se verificó el correcto funcionamiento del sitio web accediendo desde el navegador mediante la IP pública del servidor:

👉 http://34.229.200.24

El sitio carga correctamente y permite la navegación entre sus secciones.

---

## 🔐 Seguridad

* Uso de usuario personalizado (no root)
* Firewall activo (UFW)
* Configuración de permisos básicos

---

## 📎 Conclusión

Se logró implementar correctamente un servidor web en AWS EC2 con WordPress, configurando todos los componentes necesarios y resolviendo problemas durante el proceso.

El proyecto permitió comprender el despliegue de aplicaciones web en la nube, la configuración de servicios y la importancia de la seguridad en servidores.
