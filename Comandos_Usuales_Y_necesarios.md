# Comandos de linux para navegar por la terminal
## Índice  
1. [Navegación y Archivos](#1-navegación-y-archivos)
2. [Texto y Búsqueda](#2-texto-y-búsqueda)
3. [Procesos y Sistema](#3-procesos-y-sistema)
4. [Redes](#4-redes)
5. [Permisos y Usuarios](#5-permisos-y-usuarios)
6. [Discos y Almacenamiento](#6-discos-y-almacenamiento)
7. [Logs y Monitorización](#7-logs-y-monitorización)
8. [Scripting y Automatización](#8-scripting-y-automatización)
9. [Git y Control de Versiones](#9-git-y-control-de-versiones)
10. [Docker y Contenedores](#10-docker-y-contenedores)

---

## 1. Navegación y Archivos

### Rutas absolutas vs rutas relativas

#### Las rutas absolutas son:
    - /etc/apt/sources.list
    - /var/log/syslog
    - /home/ruta/destino
    - /usr/bin
#### Las rutas relativas son:
    - ../javier/documentos/
    - mis-fotos/verano.jpg
    - carta.txt
    - ../../home/alumnos/.bashrc

### Navegación básica
```bash
pwd                                         # Muestra el directorio actual
cd /ruta/destino                            # Cambia al directorio indicado
cd ..                                       # Sale hacia arriba del directorio actual
cd ~                                        # Va al directorio home del usuario
cd ~/destino                                # Permite moverse directamente a un directorio
cd -                                        # Vuelve al directorio anterior
``` 
### Listar archivos

```bash
ls                                          # Lista archivos del directorio actual
ls -l                                       # Lista con detalles (permisos, tamaño, fecha)
ls -la                                      # La a incluye los archivos ocultos
ls -lh                                      # La h incluye el tamaño en formatos legibles (KB, MB, GB)
ls -lt                                      # Ordena por fecha de modificación
ls -R                                       # Lista recursivamente
ls /ruta                                    # Lista un directorio concreto
tree                                        # Lista los elementos en forma de árbol
tree -L numero                              # Imprimere la lista con un determinado número de niveles
tree -a -C                                  # Imprime archivos ocultos coloreando la salida
tree -s -h --du                             # Iprime el tamaño de cada archivo y el tamaño total de cada directorio en formato legible para humanos
tree -I 'directorio'                        # Imprime el árbol ignorando los directorios específicados
``` 
### Crear, copiar, mover y eliminar

```bash
touch archivo.txt                           # Crea un archivo vacío o actualiza su fecha
mkdir carpeta                               # Crea un directorio
mkdir -p ruta/sub/dir                       # Crea directorios anidados
cp archivo destino                          # Copia un archivo
cp -r carpeta destino                       # Copia un directorio completo
cp -p archivo destino                       # Copia preservando permisos y fechas

mv archivo destino                          # Mueve o renombra un archivo
mv carpta/ nueva/                           # Mueve o renombra un directorio
rm archivo                                  # Elimina un archivo
rm -r carpeta                               # Elimina un directorio y su contenido
rm -rf carpeta/                             # Elimina sin preguntar nada
rmdir carpeta                               # Elimina un directorio vacío
```

### Buscar archivos con find

```bash
find / -name "archivo.txt"                  # Busca por nombre desde la raíz
find . -name "*.log"                        # Busca archivos .log en el directorio actual
find /var -type f -name "*.conf"            # Solo archivos 
find /vara type d                           # Solo directorios
find . -size +100M                          # Archivos de más de 100MB
find . -mtime -7                            # Modificados en los últimos 7 días
find . -mtime -30                           # Modificados hace más de 30 días
find . -empty                               # Archivos o directorios vacíos
find /etc -name "*.conf" -exec ls -lh {} /; # Busca y ejecuta un comando sobre cada resultado
find . -name "*.log" -delte                 # Busca y elimina directamente
```

### Ver contenido de archivos

```bash
cat archivo.txt                             # Muestra el contenido completo
cat -n archivo.txt                          # Muestra con números de línea
less archivo.txt                            # Visor paginado 
more archivo.txt                            # Visor paginado simple
head archivo.txt                            # Muestra las primeras 10 líneas
head -n 20 archivo.txt                      # Muestra las primeras 20 líneas
tail archivo.txt                            # Muestra las 10 últimas líneas
tail -n 50 archivo.txt                      # Muestra las 50 últimas líneas
tail -f arhivo.log                          # Sigue el archivo en tiempo real
```

### bat 

```bash
bat archivo.txt                             # Muestra con sintaxis coloreada y números de línea
bat -n archivo.txt                          # Solo números de línea, sin colores
bat --style=plain archivo                   # Sin decoración extra
bat *.conf                                  # Ver varios archivos seguidos
```
>!!! No suele venir instalado.

### Enlaces

```bash
tar -cfz archivo.tar.gz carpeta/            # Comprime en .tar.gz
tar -xzf archivo.tar.gz                     # Descomprime .tar.gz
tar -cfj archivo.tar.bz2                    # Comprime .tar.bz2
tar -xfj archivo.tar.bz2                    # Descomprime .tar.bz2
tar -tf archivo.tar.gz                      # Lista el contenido sin descomprimir

zip -r archivo.zip carpeta/                 # Comprime en .zip
unzip archivo.zip                           # Descomprime en .zip
unzip archivo.zip -d /destino/              # Descomprime en un directorio concreto
```

---

## 2. Texto y Búsqueda

### grep - buscar dentro de archivos

```bash
grep "texto" archivo.txt                    # Busca una cadena en un archivo
grep -i "texto" archivo.txt                 # Busqueda sin distinguir mayúsculas
grep -r "texto" /ruta/                      # Búsqueda recursiva en directorio
grep -n "texto" archivo.txt                 # Muestra número de líneas
grep -v "texto" archivo.txt                 # Muestra línas que NO contienen el texto
grep -c "texto" archivo.txt                 # Cuenta las líneas que coinciden
grep -l "texto" *.log                       # Muestra solo el nombre del archivo
grep -A 3 "error" archivo.log               # Muestra 3 líneas después del match
grep -B 3 "error" archivo.log               # Muestra 3 línas antes del match
grep -E "error|warn" archivo.log            # Búsqueda con expresiones regulares (OR)
grep "^root" /etc/passwd                    # Líneas que empiezan por "root"
grep "bash$" /etc/passwd                    # Líneas que terminan en bash

# Combinado con pipes
cat archivo.log | grep "ERROR"
journalctl | grep -i "failed"
ps aux | grep nginx
```
### sed - editar texto en flujo

```bash
sed 's/viejo/nuevo/' archivo.txt            # Sustituye primera ocurrencia por línea
sed 's/viejo/nuevo/g' archivo.txt           # Sustituye todas las ocurrencias
sed -i 's/viejo/nuevo/g' archivo.txt        # Edita el archivo directamente 
sed -i.bak 's/viejo/nuevo/g' archivo.txt    # Edita y guarda backup como .bak
sed '/texto/d' archivo.txt                  # Elimina líneas que contienen "texto"
sed -n '5,10p' archivo.txt                  # Muestra solo líneas 5 a 10
sed -n '/inicio/,/fin/p' archivo.txt        # Muestra entre dos patrones
sed 's/  */ /g' archivo.txt                 # Elimina espacios múltiples
```

### awk — procesado de columnas y datos
 
```bash
awk '{print $1}' archivo.txt                # Imprime la primera columna
awk '{print $1, $3}' archivo.txt            # Imprime columnas 1 y 3
awk -F: '{print $1}' /etc/passwd            # Usa : como separador (usuarios del sistema)
awk -F: '{print $1, $7}' /etc/passwd        # Usuario y shell
awk '{sum += $1} END {print sum}' archivo   # Suma todos los valores de la columna 1
awk 'NR==5' archivo.txt                     # Imprime solo la línea 5
awk 'NR>=5 && NR<=10' archivo.txt           # Imprime líneas 5 a 10
awk '/error/ {print $0}' archivo.log        # Imprime líneas que contienen "error"
awk '{print NF}' archivo.txt                # Imprime el número de columnas por línea
ps aux | awk '{print $1, $11}'              # Usuario y proceso de ps aux
```
### cut - extraer columnas

```bash
cut -d: -f1 /etc/passwd                     # Primer campo separado por :
cut -d, -f1,3 archivo.csv                   # Campos 1 y 3 de un CSV
cut -c1-10 archivo.txt                      # Primeros 10 caracteres de cada línea
```

### sort y uniq

```bash
sort archivo.txt                            # Ordena alfabéticamente
sort -r archivo.txt                         # Ordena en orden inverso
sort -n archivo.txt                         # Ordena númericamente
sort -k2 archivo.txt                        # Ordena por la segunda columna
sort -u archivo.txt                         # Ordena y elimina duplicados

uniq archivo.txt                            # Elimina líneas duplicadas consecutivas
uniq -c archivo.txt                         # Cuenta repeticiones
uniq -d archivo.txt                         # Muestra solo las líneas duplicadas

# Combinación habitual
sort archivo.txt | uniq -c | sort -rn       # Frecuencia de líneas, de más a menos
```
### wc-contar

```bash
wc -l archivo.txt                           # Cuenta líneas
wc -w arvhivo.txt                           # Cuenta palabras
wc -c archivo.txt                           # Cuenta bytes
wc -m archivo.txt                           # Cuenta caracteres
ls /etc | wc -l                             # Cuántos archivos hay en /etc
```

### diff y comparación

```bash
diff archivo1.txt archivo2.txt              # Compara dos archivos
diff -u archivo1.txt archivo2.txt           # Formato unificado
diff -r carpeta1/ carpeta2                  # Compara dos directorios
```

### xargs-pasar argumentos a comandos

```bash
find . -name "*log" | xargs rm              # Elimina todos los .log encontrados
find . -name "*.txt" | xargs grep "error"   # Busca "error" en todos los .txt
cat lista.txt | xargs mkdir                 # Crea directorios desde una lista
echo "uno dos tres" | xargs -n1 echo        # Ejecuta el comando con cada argumento
```

---

## 3. Procesos y Sistema

### Ver procesos

```bash
ps aux                                      # Lista todos los procesos del sistema
ps aux | grep nginx                         # Busca un proceso específico
ps -ef                                      # Formato completo con PID padre
ps -u usuario                               # Procesos de uun usuario concreto
pstree                                      # Árbol de procesos
pgrep nginx                                 # Obtiene el PID de un proceso por nombre
```

### Monitorización en tiempo real

```bash
top                                         # Monitor de procesos en tiempo real
htop                                        # Versión mejorada de top
btop                                        # Monitor avanzado con gráficas
```

### Matar procesos

```bash
kill PID                                    # Envia señal TERM
kill -9 PID                                 # Envia señal KILL
killall nginx                               # Mata todos los procesos con ese nombre
pkill -f "python script.py"                 # Mata proceo por nombre completo
```

### Prioridad de procesos

```bash
nice -n 10 comando                          # Ejecuta con prioridad reducida
nice -10 -10 comando                        # Ejecuta con prioridad alta
renice 5 -p PID                             # Cambia prioridad de un proceso en ejecución
```

### Trabajos en segundo plano

```bash
comando &                                   # Ejecuta en segundo plano 
jobs                                        # Lista trabajo en segundo plano
fg %1                                       # Trae el trabajo 1 a primer plano
bg %1                                       # Manda el trabajo 1 a segundo plano
nohup comando &                             # Ejecuta y no se interrumpe al cerrar sesión
disown %1                                   # Desvincula el trabajo de la terminal
```

### systemctl-gestión de servicios

```bash
systemctl start nginx                       # Inicia un servicio
systemctl stop nginx                        # Detiene un servicio
systemctl restart nginx                     # Reinicia un servicio
systemctl reload nginx                      # Recarga configuración sin reiniciar
systemctl status nginx                      # Estado del servicio
systemctl enable nginx                      # Activa el inicio automático
systemctl disable nginx                     # Desactiva el inicio automático
systemctl is-active nginx                   # ¿Está activo?
systemctl is-enabled nginx                  # ¿Está habilitado?
systemctl list-units --type=service         # Lista todos los servicios
systemctl list-units --failed               # Lista servicios con fallos
systemctl daemon-reload                     # Recarga los archivos de unidad
```
### Información del sistema

```bash
uname -a                                    # Información completa del kernel
uname -r                                    # Solo versión del kernel
hostname                                    # Nombre del equipo
hotnamectl                                  # Información completa del host
uptime                                      # Tiempo encendido y carga media
whoami                                      # Usuario actual
id                                          # UID, GID y grupos del usuaroi actual
last                                        # Últimos inicios de sesión
who                                         # Usuarios conectados ahora mismo
w                                           # Usuarios conectados y qué están haciendo
date                                        # Fecha y hora del sistema
timedatectl                                 # Zona horaria y sincronización NTP
```

### Recursos del sistema
 
```bash
free -h                                     # Memoria RAM y swap en formato legible
vmstat 1 5                                  # Estadísticas de memoria cada 1s, 5 veces
cat /proc/cpuinfo                           # Información detallada de la CPU
nproc                                       # Número de cores disponibles
lscpu                                       # Resumen de la CPU
lshw                                        # Hardware completo del sistema
lspci                                       # Dispositivos PCI
lsusb                                       # Dispositivos USB
```

### Apagar y reiniciar
 
```bash
shutdown -h now                             # Apaga ahora
shutdown -r now                             # Reinicia ahora
shutdown -h +10                             # Apaga en 10 minutos
reboot                                      # Reinicia el sistema
poweroff                                    # Apaga el sistema
reboot                                      # Reinicia el sistema
```
 
---
 
## 4. Redes
 
### Información de interfaces
 
```bash
ip a                                        # Muestra todas las interfaces y sus IPs
ip addr show eth0                           # Información de una interfaz concreta
ip link show                                # Estado de las interfaces (up/down)
ip link set eth0 up                         # Activa una interfaz
ip link set eth0 down                       # Desactiva una interfaz
```
 
### Rutas y tablas de enrutamiento
 
```bash
ip route                                    # Tabla de rutas
ip route show                               # Igual que el anterior
ip route add default via 192.168.1.1        # Añade una ruta por defecto
ip route del 192.168.2.0/24                 # Elimina una ruta
```
 
### Conexiones y puertos
 
```bash
ss -tuln                                    # Puertos en escucha (TCP y UDP)
ss -tulnp                                   # Igual, con el proceso que los usa
ss -s                                       # Resumen de estadísticas de sockets
netstat -tuln                               # Alternativa clásica a ss
netstat -tulnp                              # Con procesos 
netstat -an | grep ESTABLISHED              # Conexiones establecidas
lsof -i :80                                 # Qué proceso usa el puerto 80
lsof -i :443                                # Qué proceso usa el puerto 443
```
 
### Diagnóstico de conectividad
 
```bash
ping google.com                             # Comprueba conectividad básica
ping -c 4 google.com                        # Solo 4 pings
ping -i 0.5 google.com                      # Ping cada 0.5 segundos
 
traceroute google.com                       # Traza la ruta hasta el destino
tracepath google.com                        # Alternativa sin privilegios root
 
mtr google.com                              # Combinación de ping y traceroute en tiempo real
```
 
### DNS
 
```bash
nslookup google.com                         # Consulta DNS básica
dig google.com                              # Consulta DNS detallada
dig google.com MX                           # Registros de correo
dig google.com NS                           # Servidores de nombres
dig @8.8.8.8 google.com                     # Consulta usando un DNS concreto
host google.com                             # Resolución rápida
cat /etc/resolv.conf                        # DNS configurados en el sistema
```
 
### curl — transferencia HTTP
 
```bash
curl https://example.com                    # GET básico
curl -o archivo.html https://example.com    # Guarda la respuesta en archivo
curl -I https://example.com                 # Solo cabeceras HTTP
curl -X POST https://api.com/endpoint \
     -H "Content-Type: application/json" \
     -d '{"key":"value"}'                   # POST con JSON
curl -u usuario:password https://example.com # Autenticación básica
curl -L https://example.com                 # Sigue redirecciones
curl -s https://example.com                 # Modo silencioso (sin progreso)
curl -v https://example.com                 # Verbose (muestra todo)
curl --max-time 10 https://example.com      # Timeout de 10 segundos
```
 
### wget — descarga de archivos
 
```bash
wget https://example.com/archivo.tar.gz     # Descarga un archivo
wget -O nombre.tar.gz https://example.com/f # Descarga con nombre personalizado
wget -c https://example.com/archivo.tar.gz  # Continúa una descarga interrumpida
wget -q https://example.com                 # Modo silencioso
wget -r https://example.com                 # Descarga recursiva (web completa)
```
 
### SSH
 
```bash
ssh usuario@servidor                        # Conecta por SSH
ssh -p 2222 usuario@servidor                # Puerto personalizado
ssh -i ~/.ssh/clave.pem usuario@servidor    # Con clave privada
ssh -L 8080:localhost:80 usuario@servidor   # Túnel local
ssh -R 8080:localhost:80 usuario@servidor   # Túnel remoto
scp archivo.txt usuario@servidor:/destino/  # Copia archivo a servidor
scp -r carpeta/ usuario@servidor:/destino/  # Copia directorio
scp usuario@servidor:/ruta/archivo.txt .    # Descarga archivo del servidor
rsync -avz carpeta/ usuario@servidor:/destino/ # Sincronización eficiente
```
 
### Firewall (firewalld / iptables)
 
```bash
# firewalld (Fedora/RHEL)
firewall-cmd --state                        # Estado del firewall
firewall-cmd --list-all                     # Reglas activas
firewall-cmd --add-port=80/tcp --permanent  # Abre un puerto permanentemente
firewall-cmd --remove-port=80/tcp --permanent # Cierra un puerto
firewall-cmd --add-service=http --permanent # Abre un servicio por nombre
firewall-cmd --reload                       # Aplica los cambios
 
# iptables
iptables -L                                 # Lista reglas
iptables -L -n -v                           # Con detalles y sin resolver nombres
iptables -A INPUT -p tcp --dport 80 -j ACCEPT # Permite tráfico al puerto 80
iptables -A INPUT -j DROP                   # Bloquea todo lo demás
```
 
---
 
## 5. Permisos y Usuarios
 
### Permisos de archivos
 
```bash
chmod 755 archivo                           # rwxr-xr-x
chmod 644 archivo                           # rw-r--r--
chmod 600 archivo                           # rw-------
chmod +x script.sh                          # Añade permiso de ejecución
chmod -x script.sh                          # Quita permiso de ejecución
chmod -R 755 carpeta/                       # Aplica recursivamente
 
# Notación simbólica
chmod u+x archivo                           # Añade ejecución al propietario
chmod g-w archivo                           # Quita escritura al grupo
chmod o-r archivo                           # Quita lectura a otros
chmod a+r archivo                           # Añade lectura a todos
```
 
### Propietario y grupo
 
```bash
chown usuario archivo                       # Cambia propietario
chown usuario:grupo archivo                 # Cambia propietario y grupo
chown -R usuario:grupo carpeta/             # Aplica recursivamente
chgrp grupo archivo                         # Cambia solo el grupo
```
 
### Permisos especiales
 
```bash
chmod u+s archivo                           # SetUID: ejecuta como propietario
chmod g+s carpeta/                          # SetGID: hereda grupo del directorio
chmod +t carpeta/                           # Sticky bit: solo el propietario puede borrar
```
 
### Usuarios y grupos
 
```bash
useradd usuario                             # Crea un usuario
useradd -m -s /bin/bash usuario             # Con directorio home y shell
useradd -G sudo,docker usuario              # Añade a grupos al crear
passwd usuario                              # Cambia la contraseña de un usuario
userdel usuario                             # Elimina un usuario
userdel -r usuario                          # Elimina usuario y su directorio home
 
groupadd grupo                              # Crea un grupo
groupdel grupo                              # Elimina un grupo
usermod -aG grupo usuario                   # Añade usuario a un grupo existente
usermod -s /bin/bash usuario                # Cambia la shell del usuario
gpasswd -d usuario grupo                    # Elimina usuario de un grupo
 
id usuario                                  # UID, GID y grupos de un usuario
groups usuario                              # Grupos a los que pertenece
cat /etc/passwd                             # Lista de usuarios del sistema
cat /etc/group                              # Lista de grupos del sistema
```
 
### sudo
 
```bash
sudo comando                                # Ejecuta un comando como root
sudo -i                                     # Abre shell como root
sudo -u usuario comando                     # Ejecuta como otro usuario
sudo !!                                     # Repite el último comando con sudo
visudo                                      # Edita /etc/sudoers de forma segura
```
 
---
 
## 6. Discos y Almacenamiento
 
### Espacio en disco
 
```bash
df -h                                       # Espacio en todos los sistemas de archivos
df -h /                                     # Solo la partición raíz
df -i                                       # Uso de inodos
du -sh carpeta/                             # Tamaño total de una carpeta
du -sh *                                    # Tamaño de cada elemento en el directorio actual
du -sh /* 2>/dev/null | sort -rh            # Directorios raíz ordenados por tamaño
du -h --max-depth=1 /var                    # Tamaño de subdirectorios de /var
```
 
### Discos y particiones
 
```bash
lsblk                                       # Lista dispositivos de bloque
lsblk -f                                    # Con sistema de archivos y UUID
fdisk -l                                    # Lista particiones (requiere root)
fdisk /dev/sdb                              # Gestiona particiones del disco sdb
parted -l                                   # Alternativa a fdisk
blkid                                       # UUID y tipo de sistema de archivos
```
 
### Montar y desmontar
 
```bash
mount /dev/sdb1 /mnt/datos                  # Monta una partición
mount -t ext4 /dev/sdb1 /mnt/datos          # Especificando el tipo
umount /mnt/datos                           # Desmonta
umount -l /mnt/datos                        # Desmonta de forma lazy (si está ocupado)
mount | grep sdb                            # Ver qué hay montado
cat /etc/fstab                              # Montajes permanentes del sistema
```
 
### Sistemas de archivos
 
```bash
mkfs.ext4 /dev/sdb1                         # Formatea como ext4
mkfs.xfs /dev/sdb1                          # Formatea como xfs
mkfs.vfat /dev/sdb1                         # Formatea como FAT32
fsck /dev/sdb1                              # Comprueba y repara sistema de archivos
e2fsck -f /dev/sdb1                         # Comprobación forzada de ext4
tune2fs -l /dev/sdb1                        # Información del sistema de archivos ext4
```
 
### LVM (Logical Volume Manager)
 
```bash
pvs                                         # Lista Physical Volumes
vgs                                         # Lista Volume Groups
lvs                                         # Lista Logical Volumes
pvcreate /dev/sdb                           # Crea un PV
vgcreate mi_vg /dev/sdb                     # Crea un VG
lvcreate -L 10G -n mi_lv mi_vg              # Crea un LV de 10GB
lvextend -L +5G /dev/mi_vg/mi_lv            # Amplía un LV
resize2fs /dev/mi_vg/mi_lv                  # Amplía el sistema de archivos ext4
xfs_growfs /punto/montaje                   # Amplía el sistema de archivos xfs
```
 
### SMART y salud del disco
 
```bash
smartctl -a /dev/sda                        # Información completa SMART del disco
smartctl -t short /dev/sda                  # Test rápido
smartctl -H /dev/sda                        # Solo estado de salud
```
 
---
 
## 7. Logs y Monitorización
 
### journalctl — logs de systemd
 
```bash
journalctl                                  # Todos los logs del sistema
journalctl -f                               # Sigue los logs en tiempo real
journalctl -n 100                           # Últimas 100 líneas
journalctl -u nginx                         # Logs de un servicio concreto
journalctl -u nginx -f                      # Logs del servicio en tiempo real
journalctl --since "1 hour ago"             # Logs de la última hora
journalctl --since "2025-01-01"             # Logs desde una fecha
journalctl --since "2025-01-01" --until "2025-01-02"
journalctl -p err                           # Solo errores
journalctl -p warning                       # Solo warnings y superiores
journalctl -k                               # Solo logs del kernel
journalctl --disk-usage                     # Espacio que ocupan los logs
journalctl --vacuum-time=7d                 # Limpia logs de más de 7 días
journalctl -b                               # Logs del arranque actual
journalctl -b -1                            # Logs del arranque anterior
```
 
### tail y seguimiento de logs
 
```bash
tail -f /var/log/syslog                     # Sigue el log en tiempo real
tail -f /var/log/nginx/access.log           # Log de acceso de Nginx
tail -f /var/log/nginx/error.log            # Log de errores de Nginx
tail -n 200 /var/log/auth.log               # Últimas 200 líneas del log de autenticación
multitail /var/log/syslog /var/log/auth.log # Varios logs a la vez
```
 
### watch — ejecutar comandos periódicamente
 
```bash
watch df -h                                 # Actualiza df cada 2 segundos
watch -n 5 df -h                            # Actualiza cada 5 segundos
watch -d df -h                              # Resalta los cambios
watch -n 1 'ps aux | grep nginx'            # Monitoriza un proceso
watch -n 2 'ss -tuln'                       # Monitoriza puertos abiertos
```
 
### Archivos de log importantes
 
```bash
/var/log/syslog                             # Log general del sistema (Debian/Ubuntu)
/var/log/messages                           # Log general (RHEL/Fedora/CentOS)
/var/log/auth.log                           # Autenticación (Debian/Ubuntu)
/var/log/secure                             # Autenticación (RHEL/Fedora)
/var/log/kern.log                           # Logs del kernel
/var/log/dmesg                              # Mensajes del kernel al arrancar
/var/log/nginx/                             # Logs de Nginx
/var/log/apache2/                           # Logs de Apache
/var/log/postgresql/                        # Logs de PostgreSQL
/var/log/cron                               # Logs de tareas cron
```
 
### dmesg — mensajes del kernel
 
```bash
dmesg                                       # Mensajes del kernel
dmesg | tail -20                            # Últimos 20 mensajes
dmesg -T                                    # Con timestamps legibles
dmesg | grep -i error                       # Solo errores del kernel
dmesg | grep -i usb                         # Eventos USB
dmesg -w                                    # Sigue los mensajes en tiempo real
```
 
### Monitorización de rendimiento
 
```bash
iostat                                      # Estadísticas de CPU y discos
iostat -x 1                                 # Extendido, actualización cada segundo
vmstat 1                                    # Estadísticas de memoria, CPU, IO
sar -u 1 5                                  # CPU durante 5 segundos
sar -r 1 5                                  # Memoria durante 5 segundos
iotop                                       # Procesos con más IO de disco
nethogs                                     # Procesos con más uso de red
iftop                                       # Tráfico de red por interfaz
```
 
---
 
## 8. Scripting y Automatización Bash
 
### Estructura básica de un script
 
```bash
#!/bin/bash
# Descripción del script
# Autor: José Manuel
# Fecha: 2025
 
set -e                                      # Sale si algún comando falla
set -u                                      # Error si se usa variable no definida
set -o pipefail                             # Error si falla algún comando en un pipe
 
echo "Script iniciado"
```
 
### Variables
 
```bash
# Declarar variables
nombre="Jose"
numero=42
lista=(uno dos tres)
 
# Usar variables
echo "$nombre"
echo "${nombre}_apellido"                   # Con texto pegado, usar llaves
echo "${lista[0]}"                          # Primer elemento del array
echo "${#lista[@]}"                         # Número de elementos
 
# Variables de entorno
echo "$HOME"                                # Directorio home
echo "$USER"                                # Usuario actual
echo "$PATH"                                # Rutas de ejecutables
echo "$PWD"                                 # Directorio actual
echo "$SHELL"                               # Shell actual
echo "$$"                                   # PID del script actual
 
# Variables especiales en scripts
echo "$0"                                   # Nombre del script
echo "$1"                                   # Primer argumento
echo "$2"                                   # Segundo argumento
echo "$@"                                   # Todos los argumentos
echo "$#"                                   # Número de argumentos
echo "$?"                                   # Código de salida del último comando
```
 
### Condicionales
 
```bash
# if básico
if [ "$variable" == "valor" ]; then
    echo "Coincide"
elif [ "$variable" == "otro" ]; then
    echo "Es otro"
else
    echo "No coincide"
fi
 
# Comparaciones numéricas
if [ "$num" -eq 10 ]; then echo "igual"; fi
if [ "$num" -ne 10 ]; then echo "distinto"; fi
if [ "$num" -gt 10 ]; then echo "mayor"; fi
if [ "$num" -lt 10 ]; then echo "menor"; fi
if [ "$num" -ge 10 ]; then echo "mayor o igual"; fi
if [ "$num" -le 10 ]; then echo "menor o igual"; fi
 
# Comparaciones de cadenas
if [ -z "$var" ]; then echo "vacía"; fi
if [ -n "$var" ]; then echo "no vacía"; fi
if [ "$a" == "$b" ]; then echo "iguales"; fi
if [ "$a" != "$b" ]; then echo "distintas"; fi
 
# Comprobaciones de archivos
if [ -f "archivo.txt" ]; then echo "es un fichero"; fi
if [ -d "carpeta" ]; then echo "es un directorio"; fi
if [ -e "ruta" ]; then echo "existe"; fi
if [ -r "archivo" ]; then echo "tiene permisos de lectura"; fi
if [ -w "archivo" ]; then echo "tiene permisos de escritura"; fi
if [ -x "archivo" ]; then echo "tiene permisos de ejecución"; fi
if [ -s "archivo" ]; then echo "no está vacío"; fi
 
# Operadores lógicos
if [ "$a" == "x" ] && [ "$b" == "y" ]; then echo "ambas"; fi
if [ "$a" == "x" ] || [ "$b" == "y" ]; then echo "alguna"; fi
```
 
### Bucles
 
```bash
# for clásico
for i in 1 2 3 4 5; do
    echo "Número: $i"
done
 
# for con rango
for i in {1..10}; do
    echo "$i"
done
 
# for con paso
for i in {0..20..2}; do
    echo "$i"
done
 
# for sobre archivos
for archivo in /var/log/*.log; do
    echo "Procesando: $archivo"
done
 
# for estilo C
for ((i=0; i<10; i++)); do
    echo "$i"
done
 
# while
contador=0
while [ $contador -lt 5 ]; do
    echo "Contador: $contador"
    ((contador++))
done
 
# until (contrario a while)
until [ $contador -ge 5 ]; do
    echo "$contador"
    ((contador++))
done
 
# Leer líneas de un archivo
while IFS= read -r linea; do
    echo "$linea"
done < archivo.txt
```
 
### Funciones
 
```bash
# Definir función
saludar() {
    local nombre="$1"                       # Variable local
    echo "Hola, $nombre"
    return 0                                # Código de salida
}
 
# Llamar función
saludar "Jose"
 
# Función con valor de retorno
obtener_fecha() {
    echo "$(date +%Y-%m-%d)"
}
 
fecha=$(obtener_fecha)
echo "Fecha: $fecha"
```
 
### Manejo de errores
 
```bash
# Comprobar código de salida
comando
if [ $? -ne 0 ]; then
    echo "El comando falló"
    exit 1
fi
 
# Forma más limpia
if ! comando; then
    echo "El comando falló"
    exit 1
fi
 
# Función de error
error_exit() {
    echo "ERROR: $1" >&2
    exit 1
}
 
[ -f "archivo.txt" ] || error_exit "El archivo no existe"
```
 
### Entrada y salida
 
```bash
# Leer entrada del usuario
read -p "Introduce tu nombre: " nombre
read -sp "Introduce contraseña: " password   # -s oculta la entrada
echo ""
 
# Redirección
comando > archivo.txt                       # Redirige stdout (sobreescribe)
comando >> archivo.txt                      # Redirige stdout (añade)
comando 2> errores.txt                      # Redirige stderr
comando &> todo.txt                         # Redirige stdout y stderr
comando > /dev/null 2>&1                    # Descarta toda la salida
 
# Pipes
comando1 | comando2                         # Pasa stdout de 1 como stdin de 2
comando1 |& comando2                        # Pasa stdout y stderr
```
 
### Cron — tareas programadas
 
```bash
crontab -e                                  # Edita las tareas del usuario actual
crontab -l                                  # Lista las tareas actuales
crontab -r                                  # Elimina todas las tareas
 
# Formato: minuto hora día mes día_semana comando
# *  *  *  *  *  comando
# │  │  │  │  └─ 0-7 (0 y 7 = domingo)
# │  │  │  └──── 1-12 (mes)
# │  │  └─────── 1-31 (día del mes)
# │  └────────── 0-23 (hora)
# └───────────── 0-59 (minuto)
 
# Ejemplos
0 2 * * *    /scripts/backup.sh             # Cada día a las 2:00
*/5 * * * *  /scripts/check.sh              # Cada 5 minutos
0 0 * * 0    /scripts/semanal.sh            # Cada domingo a medianoche
0 9 1 * *    /scripts/mensual.sh            # El día 1 de cada mes a las 9:00
@reboot      /scripts/inicio.sh             # Al arrancar el sistema
```
 
---
 
## 9. Git y Control de Versiones
 
### Configuración inicial
 
```bash
git config --global user.name "José Manuel"
git config --global user.email "josman04con@gmail.com"
git config --global core.editor "nano"
git config --list                           # Ver configuración actual
```
 
### Repositorio
 
```bash
git init                                    # Inicia un repositorio nuevo
git clone https://github.com/user/repo      # Clona un repositorio
git clone --depth 1 https://...             # Clona solo el último commit
```
 
### Estado y cambios
 
```bash
git status                                  # Estado del repositorio
git diff                                    # Cambios no staged
git diff --staged                           # Cambios staged
git log                                     # Historial de commits
git log --oneline                           # Historial resumido
git log --oneline --graph                   # Con árbol de ramas
git log --author="Jose"                     # Commits de un autor
git show HASH                               # Detalles de un commit concreto
```
 
### Añadir y confirmar cambios
 
```bash
git add archivo.txt                         # Añade un archivo al staging
git add .                                   # Añade todos los cambios
git add -p                                  # Añade cambios de forma interactiva
git commit -m "Mensaje del commit"          # Confirma los cambios
git commit -am "Mensaje"                    # Add + commit en un paso
git commit --amend                          # Modifica el último commit
```
 
### Ramas
 
```bash
git branch                                  # Lista ramas locales
git branch -a                               # Lista todas las ramas
git branch nueva-rama                       # Crea una rama
git checkout nueva-rama                     # Cambia a la rama
git checkout -b nueva-rama                  # Crea y cambia en un paso
git switch nueva-rama                       # Alternativa moderna a checkout
git switch -c nueva-rama                    # Crea y cambia (moderno)
git branch -d rama                          # Elimina una rama
git branch -D rama                          # Elimina una rama forzado
git merge rama                              # Fusiona una rama en la actual
git rebase main                             # Rebase sobre main
```
 
### Repositorio remoto
 
```bash
git remote -v                               # Lista remotos configurados
git remote add origin URL                   # Añade un remoto
git remote remove origin                    # Elimina un remoto
git fetch                                   # Descarga cambios sin fusionar
git pull                                    # Descarga y fusiona
git pull origin main                        # Desde una rama concreta
git push                                    # Sube cambios
git push origin main                        # A una rama concreta
git push -u origin main                     # Primera vez, establece upstream
git push --force-with-lease                 # Push forzado seguro
```
 
### Deshacer cambios
 
```bash
git restore archivo.txt                     # Descarta cambios no staged
git restore --staged archivo.txt            # Quita del staging sin perder cambios
git reset HEAD~1                            # Deshace el último commit
git reset --hard HEAD~1                     # Deshace el último commit
git revert HASH                             # Crea un commit que deshace otro
git stash                                   # Guarda cambios temporalmente
git stash pop                               # Recupera los cambios guardados
git stash list                              # Lista los stashes guardados
git stash drop                              # Elimina el último stash
```
 
### Tags
 
```bash
git tag                                     # Lista tags
git tag v1.0                                # Crea un tag ligero
git tag -a v1.0 -m "Versión 1.0"            # Tag anotado
git push origin v1.0                        # Sube un tag al remoto
git push origin --tags                      # Sube todos los tags
```
 
---
 
## 10. Docker y Contenedores
 
### Imágenes
 
```bash
docker images                               # Lista imágenes locales
docker pull nginx                           # Descarga una imagen
docker pull nginx:1.25                      # Versión concreta
docker build -t mi-app:1.0 .                # Construye imagen desde Dockerfile
docker rmi nginx                            # Elimina una imagen
docker rmi $(docker images -q)              # Elimina todas las imágenes
docker image prune                          # Elimina imágenes sin usar
docker tag mi-app:1.0 usuario/mi-app:1.0    # Etiqueta una imagen
docker push usuario/mi-app:1.0              # Sube imagen a DockerHub
```
 
### Contenedores
 
```bash
docker run nginx                            # Ejecuta un contenedor
docker run -d nginx                         # En segundo plano (detached)
docker run -d -p 8080:80 nginx              # Mapea puerto host:contenedor
docker run -d --name mi-nginx nginx         # Con nombre personalizado
docker run -d -v /host/ruta:/container/ruta nginx # Monta un volumen
docker run -e MI_VAR=valor nginx            # Pasa variable de entorno
docker run --rm nginx                       # Se elimina al parar
docker run -it ubuntu bash                  # Interactivo con terminal
 
docker ps                                   # Contenedores en ejecución
docker ps -a                                # Todos los contenedores
docker stop mi-nginx                        # Para un contenedor
docker start mi-nginx                       # Inicia un contenedor parado
docker restart mi-nginx                     # Reinicia
docker rm mi-nginx                          # Elimina un contenedor parado
docker rm -f mi-nginx                       # Elimina forzado
docker rm $(docker ps -aq)                  # Elimina todos los contenedores
```
 
### Inspección y logs
 
```bash
docker logs mi-nginx                        # Logs del contenedor
docker logs -f mi-nginx                     # Logs en tiempo real
docker logs --tail 100 mi-nginx             # Últimas 100 líneas
docker inspect mi-nginx                     # Información detallada en JSON
docker stats                                # Uso de recursos en tiempo real
docker stats mi-nginx                       # De un contenedor concreto
docker top mi-nginx                         # Procesos dentro del contenedor
```
 
### Ejecutar comandos en contenedores
 
```bash
docker exec -it mi-nginx bash               # Shell interactiva en el contenedor
docker exec mi-nginx ls /etc/nginx          # Ejecuta un comando puntual
docker exec -it mi-nginx sh                 # Si no tiene bash, probar sh
docker cp archivo.txt mi-nginx:/ruta/       # Copia archivo al contenedor
docker cp mi-nginx:/ruta/archivo.txt .      # Copia archivo desde el contenedor
```
 
### Volúmenes y redes
 
```bash
docker volume ls                            # Lista volúmenes
docker volume create mi-volumen             # Crea un volumen
docker volume rm mi-volumen                 # Elimina un volumen
docker volume prune                         # Elimina volúmenes sin usar
docker volume inspect mi-volumen            # Información del volumen
 
docker network ls                           # Lista redes
docker network create mi-red                # Crea una red
docker network connect mi-red mi-nginx      # Conecta contenedor a red
docker network disconnect mi-red mi-nginx   # Desconecta
```
 
### Docker Compose
 
```bash
docker compose up                           # Levanta los servicios
docker compose up -d                        # En segundo plano
docker compose down                         # Para y elimina los contenedores
docker compose down -v                      # También elimina volúmenes
docker compose ps                           # Estado de los servicios
docker compose logs                         # Logs de todos los servicios
docker compose logs -f servicio             # Logs en tiempo real de un servicio
docker compose restart servicio             # Reinicia un servicio concreto
docker compose build                        # Construye las imágenes
docker compose pull                         # Actualiza las imágenes
docker compose exec servicio bash           # Shell en un servicio
```
 
### Dockerfile básico
 
```dockerfile
FROM ubuntu:22.04
 
LABEL maintainer="josman04con@gmail.com"
 
RUN apt-get update && apt-get install -y \
    nginx \
    curl \
    && rm -rf /var/lib/apt/lists/*
 
WORKDIR /app
 
COPY . .
 
RUN chmod +x entrypoint.sh
 
EXPOSE 80
 
ENV MI_VARIABLE=valor
 
CMD ["nginx", "-g", "daemon off;"]
```
 
### Limpieza general
 
```bash
docker system prune                         # Elimina todo lo no usado
docker system prune -a                      # Incluye imágenes sin contenedores
docker system df                            # Espacio usado por Docker
```
 
---
 
## 📝 Notas y buenas prácticas
 
- Usar `man comando` para leer la documentación oficial de cualquier comando.
- Usar `comando --help` para ver las opciones disponibles rápidamente.
- Usar `tldr comando` para ver algunas de las opciones más usadas y explicadas
- Redirigir errores a `/dev/null` con `2>/dev/null` cuando no interesen.
- En scripts, comprobar siempre el código de salida `$?` o usar `set -e`.
- Documentar siempre los scripts con comentarios explicando el propósito.