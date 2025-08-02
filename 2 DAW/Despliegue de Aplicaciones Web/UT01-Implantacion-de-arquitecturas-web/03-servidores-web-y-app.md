 
# 3. Servidores web y de aplicaciones

## 3.1 Definición y diferencias

En el contexto del desarrollo y despliegue de aplicaciones web, es importante distinguir entre dos tipos de servidores fundamentales:

### Servidor web

Un **servidor web** es un software encargado de recibir peticiones HTTP del cliente (normalmente un navegador) y devolver recursos estáticos o dinámicos, como páginas HTML, hojas de estilo, imágenes, archivos JavaScript, etc.

* **Ejemplos:** Apache HTTP Server, Nginx, LiteSpeed.
* **Funciones principales:**

  * Servir archivos estáticos.
  * Redireccionar solicitudes.
  * Gestionar certificados SSL (HTTPS).
  * Actuar como proxy inverso hacia otros servicios (por ejemplo, un backend).

### Servidor de aplicaciones

Un **servidor de aplicaciones** proporciona un entorno de ejecución para aplicaciones web dinámicas. Interpreta código del lado del servidor, gestiona sesiones, conecta con bases de datos y ejecuta lógica de negocio.

* **Ejemplos:** Apache Tomcat (Java), uWSGI (Python), PHP-FPM, Node.js.
* **Funciones principales:**

  * Ejecutar código en el servidor (Java, PHP, Python…).
  * Gestionar peticiones dinámicas (formularios, APIs).
  * Integrar capas de negocio y datos.


## 3.2 Comparación general

| Característica           | Servidor Web              | Servidor de Aplicaciones            |
| ------------------------ | ------------------------- | ----------------------------------- |
| Función principal        | Servir contenido estático | Ejecutar lógica de negocio dinámica |
| Procesamiento de scripts | No directamente           | Sí                                  |
| Lenguajes soportados     | HTML, CSS, JS             | PHP, Java, Python, etc.             |
| Ejemplos comunes         | Apache, Nginx             | Tomcat, WildFly, Node.js            |


## 3.3 Instalación y configuración básica

### 3.3.1 Ejemplo 1: Servidor web y de aplicaciones con PHP (Apache + PHP en Ubuntu)

#### Instalación del entorno LAMP (Linux + Apache + MySQL + PHP)

```bash
sudo apt update
sudo apt install apache2 php libapache2-mod-php mysql-server php-mysql
```

#### Estructura por defecto

* Document root: `/var/www/html`
* Archivos de configuración:

  * Apache: `/etc/apache2/`
  * VirtualHost: `/etc/apache2/sites-available/000-default.conf`

#### Archivo de prueba

```php
<?php
phpinfo();
?>
```

Guardar como `index.php` en `/var/www/html/`. Acceder a `http://localhost/index.php`.

#### Comandos útiles

```bash
sudo systemctl restart apache2
sudo systemctl enable apache2
```


### 3.3.2 Ejemplo 2: Servidor de aplicaciones en Java (Apache Tomcat + Spring Boot)

#### Opción A: Despliegue tradicional (WAR en Tomcat)

##### Requisitos:

* Java JDK instalado
* Tomcat descargado desde: [https://tomcat.apache.org](https://tomcat.apache.org)

```bash
# Descomprimir Tomcat
tar -xvzf apache-tomcat-<version>.tar.gz
cd apache-tomcat-<version>/bin
./startup.sh  # o startup.bat en Windows
```

##### Despliegue de aplicación

1. Crear archivo `.war` desde Spring Boot con Maven o Gradle.
2. Copiarlo al directorio `webapps/` de Tomcat.
3. La aplicación estará accesible en `http://localhost:8080/miapp`.

#### Opción B: Ejecución directa (Spring Boot empaquetado como `.jar`)

```bash
mvn clean package
java -jar target/miapp.jar
```

Spring Boot levanta su propio servidor embebido (Tomcat por defecto) en el puerto 8080.

#### Configuración del puerto y otras opciones

En `src/main/resources/application.properties`:

```properties
server.port=8081
spring.datasource.url=jdbc:mysql://localhost:3306/miapp
spring.datasource.username=usuario
spring.datasource.password=contraseña
```

## 3.4 Combinación de servidores

En arquitecturas reales, es común utilizar **ambos tipos de servidores juntos**:

* Apache o Nginx como **servidor frontal**: gestiona las peticiones del cliente, ofrece recursos estáticos, gestiona SSL, y actúa como proxy inverso.
* Tomcat, Node.js, PHP-FPM, etc. como **servidores de aplicaciones**: ejecutan la lógica y responden a las peticiones dinámicas.

```mermaid
flowchart LR
    C[Cliente navegador] --> A[Servidor web (Apache / Nginx)]
    A --> B[Servidor de aplicaciones (PHP, Tomcat, Node.js)]
    B --> D[Base de datos]
```

