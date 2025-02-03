# Proyecto 1ª evaluacion
``` bash
#!/bin/bash
# Solicitar la red a analizar
read -p "Introduce la red en formato CIDR (Ej. 192.168.1.0/24): " RED

# Obtener la parte base de la IP y la máscara
IP_BASE=$(echo $RED | cut -d'/' -f1 | awk -F. '{print $1"."$2"."$3"."}')
MASCARA=$(echo $RED | cut -d'/' -f2)

# Definir el rango de IPs basado en la máscara
if [ "$MASCARA" == "24" ]; then
    RANGO=254
elif [ "$MASCARA" == "16" ]; then
    RANGO=65534
elif [ "$MASCARA" == "8" ]; then
    RANGO=16777214
else
    echo "Máscara no soportada"
    exit 1
fi

# Archivo de salida
USER_NAME=$(whoami)
OUTPUT_FILE="reporte_${USER_NAME}_$(date +%Y%m%d_%H%M%S).txt"
echo "Análisis de red - $RED" > $OUTPUT_FILE

echo "Escaneando red..."
for i in $(seq 1 $RANGO); do
    IP="$IP_BASE$i"
    ping -c 1 -W 1 $IP &> /dev/null
    if [ $? -eq 0 ]; then
        echo "Equipo encontrado: $IP" | tee -a $OUTPUT_FILE
        
        # Obtener TTL y determinar SO
        TTL=$(ping -c 1 $IP | grep 'ttl=' | awk -F'ttl=' '{print $2}' | awk '{print $1}')
        if [ "$TTL" -le 64 ]; then
            SO="Linux"
        elif [ "$TTL" -le 128 ]; then
            SO="Windows"
        else
            SO="Desconocido"
        fi
        echo "Sistema Operativo: $SO" | tee -a $OUTPUT_FILE
        
        # Escaneo de puertos comunes
        echo "Escaneando puertos..." | tee -a $OUTPUT_FILE
        for PORT in 22 80 443 3306 3389; do
            nc -zv $IP $PORT 2>&1 | grep succeeded &> /dev/null
            if [ $? -eq 0 ]; then
                echo "  - Puerto abierto: $PORT" | tee -a $OUTPUT_FILE
            fi
        done
    fi
done

echo "Análisis finalizado. Datos guardados en $OUTPUT_FILE"
```
## 1. Pedir la red a analizar
```bash
read -p "Introduce la red en formato CIDR (Ej. 192.168.1.0/24): " RED
read -p solicita al usuario que introduzca la red en formato CIDR (Ejemplo: 192.168.1.0/24).
Se guarda en la variable RED.
```
## 2. Extraer la IP base y la máscara
```bash
IP_BASE=$(echo $RED | cut -d'/' -f1 | awk -F. '{print $1"."$2"."$3"."}')
MASCARA=$(echo $RED | cut -d'/' -f2)
Se separa la IP de la máscara usando cut -d'/' -f1 para la IP y cut -d'/' -f2 para la máscara.
awk -F. toma los primeros tres octetos y los deja listos para formar las direcciones IP.
```

## 3. Determinar el rango de IPs
```bash
Copiar
Editar
if [ "$MASCARA" == "24" ]; then
    RANGO=254
elif [ "$MASCARA" == "16" ]; then
    RANGO=65534
elif [ "$MASCARA" == "8" ]; then
    RANGO=16777214
else
    echo "Máscara no soportada"
    exit 1
fi
```
Se asigna un número de hosts según la máscara de red.


## 4. Crear un archivo de reporte
```bash
USER_NAME=$(whoami)
OUTPUT_FILE="reporte_${USER_NAME}_$(date +%Y%m%d_%H%M%S).txt"
echo "Análisis de red - $RED" > $OUTPUT_FILE
```
Se genera un archivo de salida con el nombre del usuario y la fecha/hora.

## 5. Escaneo de la red
```bash
for i in $(seq 1 $RANGO); do
    IP="$IP_BASE$i"
    ping -c 1 -W 1 $IP &> /dev/null
    if [ $? -eq 0 ]; then
seq 1 $RANGO genera los valores desde 1 hasta el límite determinado antes.
ping -c 1 -W 1 $IP envía un paquete a la IP con un timeout de 1 segundo.
```
Si la respuesta es exitosa ($? -eq 0), el host está activo.
## 6. Determinar el sistema operativo (TTL)
```bash
TTL=$(ping -c 1 $IP | grep 'ttl=' | awk -F'ttl=' '{print $2}' | awk '{print $1}')
if [ "$TTL" -le 64 ]; then
    SO="Linux"
elif [ "$TTL" -le 128 ]; then
    SO="Windows"
else
    SO="Desconocido"
fi
```
Se extrae el TTL del ping.
Se compara con valores típicos:
Linux: TTL ≤ 64
Windows: TTL ≤ 128
Si el TTL es mayor, se considera "Desconocido".
## 7. Escaneo de puertos
```bash
for PORT in 22 80 443 3306 3389; do
    nc -zv $IP $PORT 2>&1 | grep succeeded &> /dev/null
    if [ $? -eq 0 ]; then
        echo "  - Puerto abierto: $PORT" | tee -a $OUTPUT_FILE
    fi
done
```
Se escanean los puertos 22 (SSH), 80 (HTTP), 443 (HTTPS), 3306 (MySQL), 3389 (RDP) con nc (netcat).
Si el puerto está abierto, se registra en el archivo de salida.
## 8. Mensaje de finalización
```bash
echo "Análisis finalizado. Datos guardados en $OUTPUT_FILE"
```
Indica que el análisis ha terminado.