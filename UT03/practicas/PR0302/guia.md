# PR0302: Comando case
### 1: Menú de operaciones matemáticas 
```bash
#!/bin/bash
read -p "Introduce el pimer numero: " num1
read -p "Introduce el segundo numero: " num2
read -p "Introduce la operacion que quieras hacer: " operacion
case $operacion in
        suma)
                resultado=$(expr $num1 + $num2)
                echo "El resultado es $resultado";;
        resta)
                resultado=$(expr $num1 - $num2)
                echo "El resultado es $resultado";;
        multi)
                resultado=$(expr $num1 \* $num2)
                echo "El resultado es $resultado";;
        dividir)
                resultado=$(expr $num1 / $num2)
                echo "El resultado es $resultado";;
esac
```
### 2: Verificar días de la semana 
```bash
#!/bin/bash
read -p "Introduce un dia " dia
case $dia in
        lunes | martes | miercoles | jueves | viernes)
                echo "$dia es laborable";;
        domingo | sabado)
                echo "$dia es fin de semana";;
esac
```
### 3: Clasificar calificaciones 
```bash
#!/bin/bash
read -p "Introduce una nota " nota
case $nota in
        [0-4])
           echo "$nota es suspenso";;
        5)
            echo "$nota es suficiente";;
        6)
            echo "$nota es bien";;
        7 | 8)
            echo "$nota es notable";;
        9 | 10)
            echo "$nota es sobresaliente";;
esac
```
### 4: Determinar la estación del año 
```bash
#!/bin/bash
read -p "Introduce un mes: " mes
case $mes in
        [Ee]nero | [Ff]ebrero | [Dd]iciembre)
           echo "Es invierno";;
        [Mm]arzo | [Aa]bril | [Mm]ayo])
            echo "Es primavera";;
        [Ss]eptiembre | [Oo]ctubre | [Nn]oviembre)
            echo "Es otoño";;
        [Jj]unio | [Jj]ulio | [Aa]gosto)
            echo "Es verano";;
esac
```
### 5: Identificar el tipo de archivo 
```bash
#!/bin/bash
read -p "Introduce la extension de un archivo: " ext
case $ext in
        .txt)
           echo "Texto";;
        .png | .jpg | .psd | .clip)
            echo "Imagen";;
        .pdf)
            echo "Documento";;
        .mp4 | .mkv | .wav)
            echo "Video";;
esac
```
### 6: Convertir temperaturas 
```bash
#!/bin/bash
echo "Elige una conversión :"
echo "1. Celsius a Fahrenheit"
echo "2. Fahrenheit a Celsius"
echo "3. Celsius a Kelvin"
read -p "Introduce una opción (1-3): " opcion
read -p "Escribe  la temperatura: " temp

case $opcion in
  1) echo "Resultado: $(echo "$temp * 1.8 + 32" | bc -l) °F" ;;
  2) echo "Resultado: $(echo "($temp - 32) / 1.8" | bc -l) °C" ;;
  3) echo "Resultado: $(echo "$temp + 273.15" | bc -l) K" ;;
  *) echo "Opción equivocada" ;;
esac
```
### 7: Estado del servicio
```bash
#!/bin/bash
read -p "Introduce el servicio: " serv
read -p "¿Que quieres hacer con el servicio? iniciar, reiniciar o detener: " acc
case $acc in
        reiniciar)
                systemctl restart $serv
                echo "El servicio $serv se ha reiniciado";;
        iniciar)
                systemctl start $serv
                echo "El servicio $serv se ha iniciado";;
        detener)
                systemctl stop $serv
                echo "El servicio $serv se ha parado";;
esac
```
### 8: Conversión de unidades de longitud
```bash
#!/bin/bash
read -p "Introduce los metros: " num
read -p "¿A que quieres pasarlo? KM, CM o MM: " op
case $op in
       [Kk][Mm])
               resultado=$(( $num / 1000 ))
                echo "El resultado es $resultado";;
        [Cc][Mm])
               resultado=$(( $num * 100 ))
                echo "El resultado es $resultado";;
        [Mm][Mm])
               resultado=$(( $num * 1000 ))
                echo "El resultado es $resultado";;
esac
```