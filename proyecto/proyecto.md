# Proyecto 1ª evaluacion
---
# **Documentación del Proyecto de Escaneo de Red**

## **Índice**
1. Introducción    
2. Estructura del script  
   2.1. Entrada de datos  
   2.2. Funciones implementadas  
   2.3. Escaneo de red y puertos  
3. Resultados y salida  
---
## **1. Introducción**
Este script en Bash realiza un escaneo de red basado en una dirección en formato CIDR proporcionada por el usuario. Identifica dispositivos activos, determina su sistema operativo a partir del TTL y realiza un análisis de puertos abiertos, mostrando los servicios asociados. Los resultados se registran en un archivo de salida especificado por el usuario.
```bash
#!/bin/bash
# Pedir la red en formato CIDR
read -p "Introduce la ip de red en formato CIDR (ej: 192.168.1.0/24): " Red
read -p "Introduce el rango de puertos que vamos a  escanear (ej: 1-1024): " Rango
read -p "Introduce el nombre del archivo de salida: " salida
# Aqui hacemos que al introducir los datos Empiece a contar los segundos
start_time=$(date +%s)
# Iniciar archivo de salida
echo "Escaneo de red - $(date)" > "$salida"
# Función para detectar el sistema operativo basado en el TTL
function detectar_so {
    if [ "$1" -le 64 ]; then
        echo "Linux"
    elif [ "$1" -le 128 ]; then
        echo "Windows"
    else
        echo "Desconocido"
    fi
}
#  Iniciar el escaneo de la red 
echo "Escaneando la red $Red..."
for IP in $(seq 1 254); do
# Construir la dirección ip de la red que hemos dado
    TARGET="${Red%.*}.$IP"
    ping -c 1 -W 1 "$TARGET" &> /dev/null
    if [ $? -eq 0 ]; then
        echo "Equipo detectado: $TARGET"
        echo "Equipo detectado: $TARGET" >> "$salida"
        # Verificar si el host responde al ping
        # Determinar TTL y sistema Operativo
        TTL=$(ping -c 1 "$TARGET" | grep 'ttl=' | awk -F'ttl=' '{print $2}' | cut -d ' ' -f1)
        SO=$(detectar_so "$TTL")
        echo "Sistema Operativo: $SO"
        echo "Sistema Operativo: $SO" >> "$salida"
        # Escaneo de puertos
        echo "Escaneando puertos abiertos en $TARGET..."
        for PORT in $(seq $(echo $Rango | cut -d '-' -f1) $(echo $Rango | cut -d '-' -f2)); do
            nc -zv -w 1 "$TARGET" "$PORT" &> /dev/null
            if [ $? -eq 0 ]; then
                SERVICE=$(awk -F, -v port="$PORT" '$2 == port {print $3}' tcp.csv)
                echo "Puerto abierto: $PORT ($SERVICE)"
                echo "Puerto abierto: $PORT ($SERVICE)" >> "$salida"
            fi
        done
    fi
done
echo "Escaneo completado. Resultados almacenados en $salida."
# Aqui acaba de contar los segundos y te imprime en el archivo final cuanto ha tardado
end_time=$(date +%s)
echo "Tiempo de escaneo: $(($end_time - $start_time)) segundos" >> "$salida"
```
## **2. Estructura del script**

### **2.1. Entrada de datos**
El script solicita los siguientes datos al usuario:
- **Red en formato CIDR** (ejemplo: 192.168.1.0/24).
- **Rango de puertos** para escanear (ejemplo: 1-1024).
- **Nombre del archivo de salida**, donde se guardarán los resultados.

### **2.2. Funciones implementadas**
#### Función detectar_so
Determina el sistema operativo del dispositivo activo basado en el valor del TTL obtenido en la respuesta del comando ping:
- **TTL ≤ 64:** Se asume que el sistema operativo es Linux.
- **TTL ≤ 128:** Se asume que el sistema operativo es Windows.
- **TTL > 128:** Sistema operativo desconocido.

#### Tiempo de ejecución
El script mide el tiempo total de ejecución usando las marcas de tiempo (start_time y end_time) para informar al usuario cuánto tardó el análisis.

### **2.3. Escaneo de red y puertos**
1. **Detección de hosts activos:**
   - Usa ping para identificar dispositivos en la red.
   - Si un dispositivo responde, se registra en el archivo de salida y se analiza su TTL para identificar el sistema operativo.

2. **Escaneo de puertos:**
   - Usa nc para comprobar si un puerto dentro del rango especificado está abierto.
   - Si un puerto está abierto, consulta el archivo tcp.csv para identificar el servicio asociado y lo registra.

## **3. Resultados y salida**
### Archivo de salida
El archivo especificado por el usuario incluye:
- Fecha y hora del escaneo.
- Lista de dispositivos detectados con sus direcciones IP.
- Sistema operativo estimado.
- Puertos abiertos en cada dispositivo y los servicios asociados.

#### Ejemplo del archivo de salida final:
```
Escaneo de red - Tue 19 Nov 2024 08:01:01 AM UTC
Equipo detectado: 10.0.2.2
Sistema Operativo: Linux
Equipo detectado: 10.0.2.3
Sistema Operativo: Linux
Puerto abierto: 53 ("Domain Name Server")
Equipo detectado: 10.0.2.4
Sistema Operativo: Linux
Equipo detectado: 10.0.2.15
Sistema Operativo: Linux
Puerto abierto: 22 ("SSH Remote Login Protocol")
Tiempo de escaneo: 556 segundos
```
### Presentación en pantalla
Durante la ejecución, el script muestra:
- Dispositivos detectados.
- Sistema operativo detectado.
- Puertos abiertos y servicios asociados.
---