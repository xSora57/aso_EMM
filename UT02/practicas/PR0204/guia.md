# PR0204: Programación de tareas con cron
### 1. ¿Qué orden pondrías en crontab en los siguientes casos?
#### La tarea se ejecuta cada hora
"0 * * * * comando"
#### La tarea se ejecuta los domingos cada 3 horas
"0 */3 * * 0 comando"
#### La tarea se ejecuta a las 12 de la mañana los días pares del mes.
"0 12 */2 * * comando"
#### La tarea se ejecuta el primer día de cada mes a las 8 de la mañana y a las 8 de la tarde.
"0 8,20 1 * * comando"
#### La tarea se ejecuta cada media hora de lunes a viernes.
"*/30 * * * 1-5 comando"
#### La tarea se ejecuta cada cuarto de hora, entre las 3 y las 8, de lunes a viernes, durante todo el mes de agosto
"*/15 3-8 * 8 1-5 comando"
#### La tarea se ejecuta cada 90 minutos
"0 */3 * * * comando"  
"30 1-23/3 * * * comando"
### 2. ¿Cómo compruebas si el servicio cron se está ejecutando?
```bash
ps aux | grep cron
```
### 3. ¿Cuál es el efecto de la siguiente línea crontab?
```bash
*/15 1,2,3 * * * who > /tmp/test
```
Que cada 15 minutos durante las horas 1, 2 y 3 se ejecuta, fuera de estas horas no se va a ejecutar
### 4. Indica la ruta del fichero crontab del sistema
Esta en /etc/crontab
### 5. ¿Qué ficheros controlan los usuarios que pueden utilizar el crontab?
En los ficheros cron.allow y cron.deny
### 6. Excepcionalmente se debe iniciar una tarea llamada script.sh todos los lunes a las 07:30h antes de entrar en clase ¿Cómo lo harías?
30 7 * * 1 /ruta/al/script.sh
### 7. Se ha cancelado la tarea. ¿Cómo listar y luego, suprimir la tarea?
crontab -l para listar
crontab –r para eliminar
### 8. Ejecuta el comando ps -ef para el usuario root cada 2 minutos y redirecciona el resultado a /tmp/ps_result sin sobrescribir los antiguos.
*/2 * * * * sudo ps -ef >> /tmp/ps_result
### 9. Verifica la lista de tareas en crontab
crontab -l
### 10. Espera unos minutos y comprueba el resultado en /tmp
```bash
cat /tmp/ps_result
```
### 11. Crea el usuario asir2 y prohíbele utilizar el crontab.
```bash
sudo adduser asir2
echo "asir2" | sudo tee -a /etc/cron.deny
```
### 12. Verifica que el usuario asir2 realmente no puede utilizar crontab
```bash
su - asir2
crontab -e
```
### 13. Programa crontab para que cada día a las 0:05 se eliminen todos los ficheros que se encuentran en el directorio /tmp.
```bash
sudo crontab -e
5 0 * * * rm -rf /tmp/*
```
### 14.
```bash
sudo crontab -e
0 9 * 7-9 1-5 echo "$(date '+%Y-%m-%d %H:%M:%S') - $(who)" >> /ruta/a/fichero.log
```
### 15. 
cron.d: Contiene archivos de configuración de tareas cron específicas del sistema o aplicaciones, que pueden ejecutarse sin modificar el archivo crontab principal. 


cron.allow: Lista de usuarios permitidos para usar cron.


cron.deny: Lista de usuarios no permitidos para usar cron.


cron.daily: Realiza esas tareas cada dia


cron.hourly: Realiza esas tareas cada hora


cron.monthly: Realiza esas tareas cada mes