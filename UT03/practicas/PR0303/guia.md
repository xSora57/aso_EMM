# PR0303: Ejercicios sobre los comandos while, until y for
### 1. Contar hasta 10 con for
```bash
#!/bin/bash
for contador in {1..10}
do
        echo "$contador"
done
```
### 2. Sumar los primeros 50 números
```bash
#!/bin/bash
sum=0
for contador in {1..50}
do
  sum=$(( $sum + $contador ))
done
  echo $sum
```
### 3. Tabla de multiplicar
```bash
#!/bin/bash
read -p "dame un numero: " num
for contador in {1..10}
do
  res=$(( $num * $contador ))
  echo $res
done
```
### 4. Imprimir cada letra
```bash
#!/bin/bash
read -p "Introduce una palabra: " palabra
for letra  in $( echo $palabra | grep -o . )
do 
  echo $letra
done
```
### 5. Contar números pares del 1 al 20 con while:
```bash
#!/bin/bash
n=2
while [ $n -le 20 ];
 do
  echo $n
  n=$((n + 2))
done
```
### 6. Suma de dígitos
```bash
#!/bin/bash
read -p "Escribe un número: " num
suma=0
while [ $num -gt 0 ]; 
 do
  digito=$((num % 10))
  suma=$((suma + digito))
  num=$((num / 10))
done
echo "La suma de los dígitos es: $suma"
```
### 7. Cuenta regresiva
```bash
#!/bin/bash
read -p "Escribe un número: " num
until [ $num -lt 0 ]; 
 do
  echo $num
  num=$((num - 1))
done
```
### 8. Imprimir solo archivos .txt
```bash
#!/bin/bash
echo "Escribe el nombre del directorio:"
read directorio
if [ -d "$directorio" ];
        then
for archivo in *;
 do
  if [[ $archivo == *.txt ]];
         then
         echo $archivo
  fi
done
else
```
### 9. Factorial de un número
```bash
#!/bin/bash
read -p "Escribe un número: " num
factorial=1
for ((n=1; n<=num; n++));
 do
  factorial=$((factorial * n))
done
echo "El factorial de $num es: $factorial"
```
### 10. Verificar contraseña
```bash
#!/bin/bash
password="1234"
input=""
until [ "$input" == "$password" ];
 do
  read -sp "Introduce la contraseña: " input
  echo
done
echo "Contraseña correcta"
```
### 11. Adivinar un número
```bash
#!/bin/bash
numero=$((RANDOM % 10 + 1))
intento=0
while [ $intento -ne $numero ];
 do
  read -p "Adivina el número (1-10): " intento
  if [ $intento -eq $numero ];
         then
        echo "¡Correcto!"
  else
         echo "Vuelve a jugar"
  fi
done
```
### 12. Mostrar la fecha n veces
```bash
#!/bin/bash
read -p "¿Dime un número?: " n
for ((i=1; i<=n; i++)); 
 do
  date
done
```
### 13. Promedio de números ingresados
```bash
#!/bin/bash
read -p "Introduce un numero: " num
suma=0
cont=0
while [ $num != "fin" ]
do 
  suma=$(( $suma + $num ))
  cont=$(( $cont + 1 ))
  read -p  "introduce un numero:" num
done
echo "Has introducido $cont numeros"
echo  "su suma es $suma"
echo  "y su promedio es $(( $suma / $cont ))"
```
### 14. Contar palabras en una cadena
```bash
#!/bin/bash
read -p "Escribe una frase: " frase
num_palabra=$(echo $frase | wc -w)
echo "La frase tiene $num_palabra palabras"
```
### 15. Juego de adivinar con límites dinámicos
```bash
#!/bin/bash
numero=$((RANDOM % 100 + 1))
intento=0
while [ $intento -ne $numero ];
 do
  read -p "Adivina el número (1-100): " intento
  if [ $intento -lt $numero ];
         then
         echo "El número es mayor"
  elif [ $intento -gt $numero ];
         then
         echo "El número es menor"
  else
    echo "¡Correcto!"
  fi
done
```
### 16. Archivo con nombres de directorios
```bash
#!/bin/bash
ruta=$(pwd)
for f in $ruta/*
do
   if [ -d $f ]
    then
        echo $f >> directorios.txt
   fi
done
```
### 17. Generar archivos de texto numerados
```bash
#!/bin/bash
ruta=$(pwd)
for f in $ruta/; 
         do
          if [ -d $f ]
          then 
          echo $f >> ./directorios.txt
          fi
done
```
### 18. Conteo de vocales en una cadena
```bash
#!/bin/bash
read -p "Escribe una cadena: " cadena
vocales=0
for letra in $(echo $cadena | grep -o .); 
         do
         if [[ $letra =~ [aeiouAEIOU] ]]; 
                 then
                 vocales=$((vocales + 1))
         fi
         done
echo "La cadena tiene $vocales vocales"
```
### 19. Validación de entrada
```bash
#!/bin/bash
numero=0
until [[ $numero -ge 1 && $numero -le 10 ]]; 
 do
  read -p "Introduce un número entre 1 y 10: " numero
done
echo "Número válido: $numero"
```
### 20. Script de copia de seguridad
```bash
#!/bin/bash
mkdir -p backup
for archivo in *.txt; 
 do
  if [ -f "$archivo" ]; 
         then
         cp "$archivo" backup/
  fi
done
echo "Archivos .txt copiados a la carpeta backup"
```