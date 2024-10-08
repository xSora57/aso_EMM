# PR0201

## 1. Permisos de usuarios
### 1.1 Para crear las carpetas ponemos el comando:
``` bash
vagrant@ubuntu2204:~$ mkdir pr0201
vagrant@ubuntu2204:~$ cd pr0201/
vagrant@ubuntu2204:~/pr0201$ mkdir dir1
vagrant@ubuntu2204:~/pr0201$ mkdir dir2
vagrant@ubuntu2204:~/pr0201$ ls -l
total 8
drwxrwxr-x 2 vagrant vagrant 4096 Oct  1 07:08 dir1
drwxrwxr-x 2 vagrant vagrant 4096 Oct  1 07:00 dir2
```
Tienen todos los permisos el usuario y el grupo y los demas solo pueden leer y ejecutar
### 1.2 y 1.3 Modificar permisos
```bash
vagrant@ubuntu2204:~/pr0201$ chmod a-w dir2
vagrant@ubuntu2204:~/pr0201$ chmod 551 dir2
```
### 1.4 Ver permisos
```bash
vagrant@ubuntu2204:~/pr0201$ ls -l
total 8
drwxrwxr-x 2 vagrant vagrant 4096 Oct  1 07:08 dir1
dr-xr-x--x 2 vagrant vagrant 4096 Oct  1 07:00 dir2
```
### 1.5 Intentando crear directorio dir21
```bash
vagrant@ubuntu2204:~/pr0201$ cd dir2
vagrant@ubuntu2204:~/pr0201/dir2$ mkdir dir21
mkdir: cannot create directory ‘dir21’: Permission denied
```
### 1.6 Nos damos permisos de escritura y volvemos a intentarlo
```bash
vagrant@ubuntu2204:~/pr0201$ chmod u+w dir2
vagrant@ubuntu2204:~/pr0201$ cd dir2
vagrant@ubuntu2204:~/pr0201/dir2$ mkdir dir21
vagrant@ubuntu2204:~/pr0201/dir2$ ls
dir21
```
## 2. Notación octal y simbólica
### 2.1 Notación simbolica
```bash
rwxrwxr-x : chmod u+rwx,g+rwx,o+rx file
rwxr--r-- : chmod u+x,g-rw,o+r file
r--r----- : chmod u-w,g-rw,o-r file
rwxr-xr-x : chmod u+x,g+rx,o+x file
r-x--x--x : chmod u-w,g-r,o-rw,u+x,g+x,o+x file
-w-r----x : chmod u-wx,g+w,o+x,o-r file
-----xrwx : chmod u-rw,g-rw,o+rwx,g+x file
r---w---x : chmod u+r,g-w,o+x,g-r file
-w------- : chmod u-w,g-r,o-r file
rw-r----- : chmod u+rw,g+r,o-r file 
rwx--x--x : chmod u+x,g-x,o+x file
```
### 2.2 Notación octal
``` bash
rwxrwxrwx : chmod 777 file
--x--x--x : chmod 111 file
r---w---x : chmod 421 file
-w------- : chmod 200 file
rw-r----- : chmod 640 file
rwx--x--x : chmod 711 file
rwxr-xr-x : chmod 755 file
r-x--x--x : chmod 511 file
-w-r----x : chmod 241 file
-----xrwx : chmod 017 file
```
## 3 El bit setgid

### 3.1 Creacion de grupo y de usuarios
```bash
vagrant@ubuntu2204:/$ sudo groupadd asir
vagrant@ubuntu2204:/$ sudo useradd -g asir emm1
vagrant@ubuntu2204:/$ sudo useradd -g asir emm2
```
### 3.2 Crear el directorio 
```bash
vagrant@ubuntu2204:/$ sudo mkdir /compartido
vagrant@ubuntu2204:/$ sudo chown root:asir /compartido
```
### 3.3 Asignar permisos al directorio
```bash
vagrant@ubuntu2204:/$ sudo chmod 770 /compartido
vagrant@ubuntu2204:/$ ls -l 
total 2097224
lrwxrwxrwx   1 root root          7 Aug 10  2023 bin -> usr/bin
drwxr-xr-x   4 root root       4096 Jan 11  2024 boot
drwxrwx---   2 root asir       4096 Oct  3 07:56 compartido
```
### 3.4 Establecer el bit setgid en el directorio
```bash
vagrant@ubuntu2204:/$ sudo chmod g+s /compartido
vagrant@ubuntu2204:/$ ls -ld compartido/
drwxrws--- 2 root asir 4096 Oct  3 07:56 compartido/
```
### 3.5
```bash
vagrant@ubuntu2204:/$ su emm1
Password:
$ cd compartido
$ touch fichero1
$ ls -l
total 0
-rw-r--r-- 1 emm1 asir 0 Oct  7 10:12 fichero1
$ echo "hola" > fichero1
$ cat fichero1
hola
```
### 3.6
```bash
vagrant@ubuntu2204:/$ su emm2
Password:
$ cd compartido
$ cat fichero1
hola
$
```
### 3.7
¿Qué ventajas tiene usar el bit setgid en entornos colaborativos?


¿Qué sucede si no se aplica el bit setgid en un entorno colaborativo?

### 3.8
```bash
vagrant@ubuntu2204:/$ sudo rm -r compartido
vagrant@ubuntu2204:/$ sudo userdel  emm1
vagrant@ubuntu2204:/$ sudo userdel  emm2
```

## 4. El sticky bit
### 4.1 Crear directorio
```bash
vagrant@ubuntu2204:/$ sudo mkdir /compartido
vagrant@ubuntu2204:/$ sudo chmod 777 /compartido
vagrant@ubuntu2204:/$ ls -ld /compartido/
drwxrwxrwx 2 root root 4096 Oct  7 10:53 /compartido/
```
### 4.2 Crear los usuarios
```bash
sudo adduser emm2
Adding user `emm2' ...
Adding new group `emm2' (1003) ...
Adding new user `emm2' (1002) with group `emm2' ...
The home directory `/home/emm2' already exists.  Not copying from `/etc/skel'.
adduser: Warning: The home directory `/home/emm2' does not belong to the user you are currently creating.  
New password:
Retype new password:
passwd: password updated successfully
Changing the user information for emm2
Enter the new value, or press ENTER for the default
        Full Name []:
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
Is the information correct? [Y/n] y
```
### 4.3 Probar el comportamiento sin el sticky bit
Primero creamos un archivo con el primer usuario
```bash
emm1@ubuntu2204:/$ cd compartido/
emm1@ubuntu2204:/compartido$ touch fichero
```
ahora con el segundo usuario intentamos borrarlo
```bash
vagrant@ubuntu2204:/$ su emm2
Password:
emm2@ubuntu2204:/$ cd compartido/
emm2@ubuntu2204:/compartido$ rm fichero
rm: remove write-protected regular empty file 'fichero'? y
emm2@ubuntu2204:/compartido$ ls
```
### 4.4 Establecer el sticky bit en el directorio
```bash
vagrant@ubuntu2204:/$ sudo chmod +t /compartido/
vagrant@ubuntu2204:/$ ls -ld compartido
drwxrwxrwt 2 root root 4096 Oct  8 07:03 compartido
```
### 4.5 Probar el comportamiento con el sticky bit
```bash
vagrant@ubuntu2204:/$ su emm1
Password:
emm1@ubuntu2204:/$ cd compartido/
emm1@ubuntu2204:/compartido$ touch fichero
emm1@ubuntu2204:/compartido$ echo hola > "fichero" 
emm1@ubuntu2204:/compartido$ cat fichero
hola
emm1@ubuntu2204:/compartido$ exit
vagrant@ubuntu2204:/$ su emm2
Password:
emm2@ubuntu2204:/$ cd compartido
emm2@ubuntu2204:/compartido$ rm fichero 
rm: remove write-protected regular file 'fichero'? y
rm: cannot remove 'fichero': Operation not permitted
```
### 4.6 Preguntas
#### ¿Qué efecto tiene el sticky bit en un directorio?
 El sticky bit impide que los usuarios borren archivos de otros, incluso si tienen permisos para escribir en el directorio. Solo el dueño del archivo, el dueño del directorio o el administrador (root) pueden borrar archivos
#### Si tienes habilitado el sticky bit, ¿cómo tendrías que hacer para eliminar un fichero dentro del directorio?
- Eres el dueño del archivo.
- Eres el dueño del directorio.
- O tienes permisos de administrador (root).