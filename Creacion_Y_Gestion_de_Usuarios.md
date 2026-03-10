# Documentación para crear usuarios, grupos y su gestión en Linux
## Usuarios
1. Crear y darle permisos:  
    ```sudo adduser nombre de usuario```
    ```sudo adduser nombre de usuario nombre del grupo```
    *Ejemplo: ```sudo adduser JMCongz02 sudo```* 
2. Crea un nuevo usuario sin un directorio home:
   ```sudo adduser --no-create-home usuario```  
3. Crea un nuevo usuario con un directorio home en la ruta especificada:
    ```sudo adduser --home ruta/a/home usuario```  
4. Crea un nuevo usuario con el intérprete de comandos especificado establecido como intérprete de comandos de inicio de sesión:
    ```sudo adduser --shell ruta/a/shell usuario```  
5. Crea un nuevo usuario que pertenezca al grupo especificado:
    ```sudo adduser --ingroup group usuario```  
### Opciones importantes a tener en cuenta
    - --expiredate: Indica una fecha de caducidad de la cuenta
    - --inactive: Indica un periodo de inactividad de la contraseña
    - --gid: Para añadir el grupo primario  
    - --groups: Para añadir más grupos adicionales  
    - --no-user-group: No crea un grupo con el mismo nombre que el del usuario
    - --password: Crea una contraseña cifrada de la nueva cuenta
    - --system: Crea una cuenta del sistema
    - --root: Establece DIR_CHROOT como el directorio
    - --uid: Identificador del usuario de la nueva cuenta
    - --user-group: Crea un grupo con el mismo nombre que el del usuario
## Grupos
1. Crear un grupo:  
    ```sudo addgroup nombregrupo```  
    *Ejemplo: ```sudo addgroup administradores```*
2. Crear grupo del sistema:  
    ```sudo addgroup --system nombre del grupo```  
3. Grupo con GID específico:  
    ```sudo addgroup --gid xxxx(Número cualquiera) nombredelgrupo ```  
4. Comprobar grupos:  
    ```groups nombreusuario```  
    ``` id nombreusuario```  
    ``` getent group nombregrupo``` (Miembros de un grupo)  
## Borrar usuarios y grupos
1. Usuarios:  
    ```sudo deluser nombreusuario```
2. Grupos:
    ```sudo delgroup nombre del grupo```  
>!!!También existen useradd, groupadd, userdel, groupdel y usermod. Se suelen usar más para comandos en scripts y a bajo nivel. Siempre está disponible usar --help o man *comando* (tambien se puede usar tldr) para ver las distintas opciones yo he puesto las más **usadas y comunes** 