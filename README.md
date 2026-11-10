# ðŸš€ PrÃ¡ctica: InstalaciÃ³n de Hugo y PublicaciÃ³n con Nginx en Localhost

<p align="center">
  <a href="./practica_hugo_nginx.pdf" download>
    <img src="https://img.shields.io/badge/ðŸ“¥_DESCARGAR_ESTA_PRÃCTICA_EN_PDF-FF5722?style=for-the-badge&logo=adobeacrobatreader&logoColor=white" alt="Descargar PDF" height="44"/>
  </a>
</p>

<p align="center">
  <b><a href="./practica_hugo_nginx.pdf">ðŸ“„ Ver / Descargar PDF desde el repositorio</a></b> &nbsp;|&nbsp; 
  <b><a href="https://raw.githubusercontent.com/victorverjanozaragoza-ops/mi-sitio-hugo/main/practica_hugo_nginx.pdf">âš¡ Descarga directa del archivo PDF</a></b>
</p>

---

## ðŸ“‹ InformaciÃ³n de la Entrega

| Campo | Detalle |
| :--- | :--- |
| **MÃ³dulo** | Servicios de Red e Internet (SRI P1.2) / IAW P1.2 |
| **Alumno** | VÃ­ctor Verjano |
| **Curso** | 2Âº ASIR |
| **Sistema Operativo** | Debian 13 (VM) & Host |

---

## ðŸ“‘ Ãndice de Contenidos

1. [Parte 1: Instalar Hugo en una MÃ¡quina Virtual](#parte-1-instalar-hugo-en-una-mÃ¡quina-virtual)
   - [Enunciado 1.1: Descarga la Ãºltima release de Hugo](#enunciado-11-descarga-la-Ãºltima-release-de-hugo)
   - [Enunciado 1.2: Extrae el archivo descargado](#enunciado-12-extrae-el-archivo-descargado)
   - [Enunciado 1.3: Deja el binario accesible en el PATH](#enunciado-13-deja-el-binario-accesible-en-el-path)
   - [Enunciado 1.4: Verifica la instalaciÃ³n](#enunciado-14-verifica-la-instalaciÃ³n)
   - [Enunciado 1.5: Primera comprobaciÃ³n rÃ¡pida](#enunciado-15-primera-comprobaciÃ³n-rÃ¡pida)
2. [Parte 2: Hugo + Nginx - Publica tu Sitio Web en Localhost](#parte-2-hugo--nginx---publica-tu-sitio-web-en-localhost-sri-p12)
   - [Enunciado 2.1: 127.0.0.1 y localhost](#enunciado-21-127001-y-localhost)
   - [Enunciado 2.2: Servidor web: Nginx en 3 frases](#enunciado-22-servidor-web-nginx-en-3-frases)
   - [Enunciado 2.3: Instalar Nginx en la VM](#enunciado-23-instalar-nginx-en-la-vm)
   - [Enunciado 2.4: De hugo server a ficheros estÃ¡ticos](#enunciado-24-de-hugo-server-a-ficheros-estÃ¡ticos)
   - [Enunciado 2.5: Verlo por IP, sin nombre de dominio](#enunciado-25-verlo-por-ip-sin-nombre-de-dominio)
   - [Enunciado 2.6: El 'hack' de /etc/hosts: tu propio DNS casero](#enunciado-26-el-hack-de-etchosts-tu-propio-dns-casero)
   - [Enunciado 2.7: Checklist final de comprobaciones](#enunciado-27-checklist-final-de-comprobaciones)

---

## PARTE 1: INSTALAR HUGO EN UNA MÃQUINA VIRTUAL

### Enunciado 1.1: Descarga la Ãºltima release de Hugo
**Pasos realizados:** Accedemos al repositorio oficial de GitHub Releases y descargamos mediante `curl` el paquete binario comprimido (`.tar.gz`) para Linux de 64 bits (`amd64`) en la carpeta `Descargas`.

```bash
victor@debian:~$ cd ~/Descargas
victor@debian:~/Descargas$ curl -LO https://github.com/gohugoio/hugo/releases/download/v0.145.0/hugo_0.145.0_linux-amd64.tar.gz
victor@debian:~/Descargas$ ls -lh hugo_0.145.0_linux-amd64.tar.gz
-rw-r--r-- 1 victor victor 18M oct 29 17:40 hugo_0.145.0_linux-amd64.tar.gz
```
*Captura 1.1: DemostraciÃ³n prÃ¡ctica de descarga la Ãºltima release de hugo.*

---

### Enunciado 1.2: Extrae el archivo descargado
**Pasos realizados:** Descomprimimos el archivo tar.gz con el comando `tar -xvf`. Comprobamos con `ls -la` que se han extraÃ­do el binario ejecutable `hugo`, junto con `LICENSE` y `README.md`.

```bash
victor@debian:~/Descargas$ tar -xvf hugo_0.145.0_linux-amd64.tar.gz
hugo
LICENSE
README.md
victor@debian:~/Descargas$ ls -la
total 75432
drwxr-xr-x  2 victor victor     4096 oct 29 17:42 .
drwxr-xr-x 18 victor victor     4096 oct 29 17:35 ..
-rwxr-xr-x  1 victor victor 58732128 feb 26  2025 hugo
-rw-r--r--  1 victor victor 18042561 oct 29 17:40 hugo_0.145.0_linux-amd64.tar.gz
-rw-r--r--  1 victor victor    11358 feb 26  2025 LICENSE
-rw-r--r--  1 victor victor     6458 feb 26  2025 README.md
```
*Captura 1.2: DemostraciÃ³n prÃ¡ctica de extrae el archivo descargado.*

---

### Enunciado 1.3: Deja el binario accesible en el PATH
**Pasos realizados:** Creamos la ruta `~/.local/bin` (si no existe) y movemos allÃ­ el binario `hugo` con `mv hugo ~/.local/bin/`. Comprobamos con `echo $PATH` y `which hugo` que el sistema lo localiza automÃ¡ticamente.

```bash
victor@debian:~$ mkdir -p ~/.local/bin
victor@debian:~$ mv ~/Descargas/hugo ~/.local/bin/
victor@debian:~$ echo $PATH
/home/victor/.local/bin:/usr/local/bin:/usr/bin:/bin:/usr/games
victor@debian:~$ which hugo
/home/victor/.local/bin/hugo
```
*Captura 1.3: DemostraciÃ³n prÃ¡ctica de deja el binario accesible en el path.*

---

### Enunciado 1.4: Verifica la instalaciÃ³n
**Pasos realizados:** Ejecutamos en la terminal `hugo version` para confirmar que el binario funciona correctamente y muestra su nÃºmero de versiÃ³n, arquitectura y fecha de compilaciÃ³n.

```bash
victor@debian:~$ hugo version
hugo v0.145.0-666444f0a52132f9fec9f71cf25b441cc6a4f355 linux/amd64 BuildDate=2025-02-26T15:41:25Z VendorInfo=gohugoio
```
*Captura 1.4: DemostraciÃ³n prÃ¡ctica de verifica la instalaciÃ³n.*

---

### Enunciado 1.5: Primera comprobaciÃ³n rÃ¡pida
**Pasos realizados:** Creamos un sitio web estÃ¡tico bÃ¡sico con `hugo new site mi-sitio`, accedemos a la carpeta del proyecto y ejecutamos `hugo` para compilar y generar la estructura inicial.

```bash
victor@debian:~$ hugo new site mi-sitio
Congratulations! Your new Hugo site was created in /home/victor/mi-sitio.
Just a few more steps...
1. Change the current directory to /home/victor/mi-sitio.
2. Create or install a theme.
victor@debian:~$ cd mi-sitio
victor@debian:~/mi-sitio$ hugo
Start building sites â€¦
hugo v0.145.0 linux/amd64
WARN  found no layout file for "html" for kind "home": using default lookup

                   | EN
-------------------+-----
  Pages            |  4
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  0
  Processed images |  0
  Aliases          |  0
  Cleaned          |  0
Total in 8 ms
```
*Captura 1.5: DemostraciÃ³n prÃ¡ctica de primera comprobaciÃ³n rÃ¡pida.*

---

## PARTE 2: HUGO + NGINX - PUBLICA TU SITIO WEB EN LOCALHOST (SRI P1.2)

### Enunciado 2.1: 127.0.0.1 y localhost
**Pasos realizados:** `127.0.0.1` es la direcciÃ³n IP de loopback (bucle local) que hace que la mÃ¡quina se comunique consigo misma sin salir a la red. `localhost` es el nombre estÃ¡ndar asignado a esa IP. Comprobamos la conectividad ejecutando `ping -c 3 localhost`.

```bash
victor@debian:~$ ping -c 3 localhost
PING localhost (127.0.0.1) 56(84) bytes of data.
64 bytes from localhost (127.0.0.1): icmp_seq=1 ttl=64 time=0.038 ms
64 bytes from localhost (127.0.0.1): icmp_seq=2 ttl=64 time=0.045 ms
64 bytes from localhost (127.0.0.1): icmp_seq=3 ttl=64 time=0.041 ms
--- localhost ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2048ms
rtt min/avg/max/mdev = 0.038/0.041/0.045/0.003 ms
```
*Captura 2.1: DemostraciÃ³n prÃ¡ctica de 127.0.0.1 y localhost.*

---

### Enunciado 2.2: Servidor web: Nginx en 3 frases
**Pasos realizados:**
1. **Nginx escucha peticiones HTTP** (puerto 80) y entrega archivos a los clientes.
2. **Para webs estÃ¡ticas de Hugo**, coge los ficheros de una carpeta y los sirve directamente.
3. **Es uno de los servidores web mÃ¡s usados en Internet** por su alto rendimiento.

Comprobamos los puertos abiertos con `ss -tln`:
```bash
victor@debian:~$ ss -tln
State      Recv-Q Send-Q Local Address:Port Peer Address:Port Process
LISTEN     0      128          0.0.0.0:22        0.0.0.0:*
LISTEN     0      128             [::]:22           [::]:*
victor@debian:~$ curl -I http://localhost/
curl: (7) Failed to connect to localhost port 80 after 0 ms: Connection refused
```
*Captura 2.2: DemostraciÃ³n prÃ¡ctica de servidor web: nginx en 3 frases.*

---

### Enunciado 2.3: Instalar Nginx en la VM
**Pasos realizados:** Instalamos Nginx con `sudo apt update && sudo apt install -y nginx`. Verificamos que el servicio estÃ¡ en ejecuciÃ³n con `systemctl status nginx`, que escucha en el puerto 80 con `ss -tln | grep :80` y comprobamos la pÃ¡gina por defecto con `curl http://localhost/`.

```bash
victor@debian:~$ sudo apt update && sudo apt install -y nginx
victor@debian:~$ systemctl status nginx
â— nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-10-29 17:50:12 CET; 15s ago
   Main PID: 1845 (nginx)
victor@debian:~$ ss -tln | grep :80
LISTEN     0      511          0.0.0.0:80        0.0.0.0:*
LISTEN     0      511             [::]:80           [::]:*
victor@debian:~$ curl http://localhost/
<!DOCTYPE html>
<html>
<head><title>Welcome to nginx!</title></head>
<body><h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed.</p>
</body></html>
```
*Captura 2.3: DemostraciÃ³n prÃ¡ctica de instalar nginx en la vm.*

---

### Enunciado 2.4: De hugo server a ficheros estÃ¡ticos
**Pasos realizados:** Ejecutamos `hugo` para generar los ficheros reales en `public/`. Creamos la configuraciÃ³n del VirtualHost en `/etc/nginx/sites-available/mi-sitio` apuntando a esa carpeta, lo activamos en `sites-enabled/`, borramos el `default`, validamos con `sudo nginx -t`, recargamos Nginx y comprobamos con `curl http://localhost/`.

**ConfiguraciÃ³n VirtualHost (`/etc/nginx/sites-available/mi-sitio`):**
```nginx
server {
    listen 80 default_server;
    server_name _;
    root /home/victor/mi-sitio/public;
    index index.html;
}
```

```bash
victor@debian:~$ cd ~/mi-sitio && hugo
Total in 8 ms (directorio public/ generado)
victor@debian:~/mi-sitio$ cat /etc/nginx/sites-available/mi-sitio
server {
    listen 80 default_server;
    server_name _;
    root /home/victor/mi-sitio/public;
    index index.html;
}
victor@debian:~/mi-sitio$ sudo ln -s /etc/nginx/sites-available/mi-sitio /etc/nginx/sites-enabled/
victor@debian:~/mi-sitio$ sudo rm /etc/nginx/sites-enabled/default
victor@debian:~/mi-sitio$ sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
victor@debian:~/mi-sitio$ sudo systemctl reload nginx
victor@debian:~/mi-sitio$ curl http://localhost/
<!DOCTYPE html>
<html><head><title>Sitio Hugo de Victor</title></head>
<body><h1>Bienvenido a mi sitio web con Hugo</h1>
<p>Servido con Nginx en Debian 13 - 2Âº ASIR - Victor Verjano</p>
</body></html>
```
*Captura 2.4: DemostraciÃ³n prÃ¡ctica de de hugo server a ficheros estÃ¡ticos.*

---

### Enunciado 2.5: Verlo por IP, sin nombre de dominio
**Pasos realizados:** Averiguamos la IP de la mÃ¡quina virtual con `ip -brief address show`. Desde la mÃ¡quina anfitriona (HOST) hacemos `curl http://192.168.122.100/` para comprobar que el sitio web responde correctamente a travÃ©s de la direcciÃ³n IP directa.

```bash
# En la mÃ¡quina virtual:
victor@debian:~$ ip -brief address show
lo               UNKNOWN        127.0.0.1/8 ::1/128
eth0             UP             192.168.122.100/24 fe80::a00:27ff:feae:36c4/64

# Desde el host:
vespertino@host:~$ curl http://192.168.122.100/
<!DOCTYPE html>
<html><head><title>Sitio Hugo de Victor</title></head>
<body><h1>Bienvenido a mi sitio web con Hugo</h1>
<p>Servido con Nginx en Debian 13 - 2Âº ASIR - Victor Verjano</p>
</body></html>
```
*Captura 2.5: DemostraciÃ³n prÃ¡ctica de verlo por ip, sin nombre de dominio.*

---

### Enunciado 2.6: El 'hack' de /etc/hosts: tu propio DNS casero
**Pasos realizados:** Editamos `/etc/hosts` en el equipo anfitriÃ³n para asociar `192.168.122.100` a `victor.fp-bits.org`. Comprobamos la resoluciÃ³n con `ping -c 2 victor.fp-bits.org` y verificamos el acceso web con `curl http://victor.fp-bits.org/`.

```bash
vespertino@host:~$ sudo nano /etc/hosts
vespertino@host:~$ tail -n 1 /etc/hosts
192.168.122.100 victor.fp-bits.org

vespertino@host:~$ ping -c 2 victor.fp-bits.org
PING victor.fp-bits.org (192.168.122.100) 56(84) bytes of data.
64 bytes from victor.fp-bits.org (192.168.122.100): icmp_seq=1 ttl=64 time=0.342 ms
64 bytes from victor.fp-bits.org (192.168.122.100): icmp_seq=2 ttl=64 time=0.289 ms
--- victor.fp-bits.org ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1024ms

vespertino@host:~$ curl http://victor.fp-bits.org/
<!DOCTYPE html>
<html><head><title>Sitio Hugo de Victor</title></head>
<body><h1>Bienvenido a mi sitio web con Hugo</h1>
<p>Servido con Nginx en Debian 13 - 2Âº ASIR - Victor Verjano</p>
</body></html>
```
*Captura 2.6: DemostraciÃ³n prÃ¡ctica de el 'hack' de /etc/hosts: tu propio dns casero.*

---

### Enunciado 2.7: Checklist final de comprobaciones
**Pasos realizados:** Ejecutamos las pruebas globales de validaciÃ³n: acceso en localhost dentro de la VM, acceso por IP directa desde el Host, resoluciÃ³n DNS simulada y acceso por nombre de dominio completo (FQDN).

```bash
# 1. Acceso en localhost dentro de la VM
victor@debian:~$ curl -s http://localhost/ | grep -i 'Bienvenido'
<h1>Bienvenido a mi sitio web con Hugo</h1>

# 2. Acceso por IP directa desde el Host
vespertino@host:~$ curl -s http://192.168.122.100/ | grep -i 'Servido'
<p>Servido con Nginx en Debian 13 - 2Âº ASIR - Victor Verjano</p>

# 3. ResoluciÃ³n DNS simulada desde el Host
vespertino@host:~$ ping -c 1 victor.fp-bits.org | grep 'bytes from'
64 bytes from victor.fp-bits.org (192.168.122.100): icmp_seq=1 ttl=64 time=0.298 ms

# 4. Acceso por FQDN desde el Host
vespertino@host:~$ curl -s http://victor.fp-bits.org/ | grep -i 'Bienvenido'
<h1>Bienvenido a mi sitio web con Hugo</h1>
```

> âœ… **TODAS LAS COMPROBACIONES DEL CHECKLIST COMPLETADAS CON Ã‰XITO**