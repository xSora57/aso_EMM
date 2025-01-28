# PR0301: Condicional if
### 1: Comprobación de número par o impar
```bash
#!/bin/bash
read -p "Ingresa un numero" numero
if ( $numero % 2 == 0 ); then
    echo "El número $numero es par."
else
    echo "El número $numero es impar."
fi
```
### 2: Verificación de archivo
```bash
#!/bin/bash
read -p "Ingresa una ruta " ruta
if [ -e $ruta ]; then
        if [ -r $ruta ]; then
           echo "el archivo $ruta exite y tiene permisos de lectura"
        else
           echo "el archivo $ruta existe pero no tiene permisos de lectura"
        fi
else
        echo "no existe el archivo"
fi
```
### 3: Comparación de dos números
```bash
#!/bin/bash
read -p "Ingresa un numero: " numero
read -p "Ingresa otro numero: " numero2
if ( numero > numero2 ); then
    echo "El número $numero es mayor que $numero2"
elif ( numero < numero2 ); then
    echo "El número $numero2 es mayor que $numero"
else
    echo "Ambos números son iguales"
fi
```
### 4: Validación de contraseña
```bash
#!/bin/bash
contcorrect="1234"
read -p "Introduce la contraseña " passwd
if [ $passwd == $contcorrect ]; then
echo "la contraseña es correcta"
else
echo "La contraseña es incorrecta"
fi
```
### 5: Comprobación de directorio
```bash
#!/bin/bash
read -p "Introduce el directorio: " directorio
if [ -e $directorio ]
then
        if [ -r $directorio ]
        then
           echo "El directorio existe y tiene permisos de lectura"
        else
           echo "El directorio existe pero no tiene permisos de lectura"
        fi
else
        $(mkdir $directorio)
        echo "Se ha creado $directorio"
fi
```
### 6: Verificar si el usuario es root
```bash
#!/bin/bash
if [[ "$USER" -ne 0 ]];
        then
                echo "El script no se está ejecutando por el usuario root."
        else
                echo  "El script si está siendo  ejecutado por el usuario root."
fi
```
### 7: Calificación de un examen
```bash
#!/bin/bash
echo "Introduce la nota del examen (de 0 a 10):"
read nota

if ((nota >=5 ));
        then
                echo "El examen está aprobado"
        else 
                echo "El examen está suspenso"
fi
```
### 8: Comprobación del espacio en disco
```bash
#!/bin/bash
espacio-libre=$(df / | grep '/' | awk '{print $5}' | sed 's/%//' )

if  (( espacio-libre < 10 ));
        then
                echo "CUIDADO!!: El espacioi del disco es menor al 10%"
        else
                echo "Tiene suficiente espacio en el disco"
fi
```
### 9: Menú de opciones
```bash
  GNU nano 6.2                                                                 menu-opciones.sh                                                                          
#!/bin/bash
echo "Elige un número:"
echo "1."
echo "2."
echo "3."
read numero
if [ "$numero" -eq 1 ];
        then
                echo "Has elegido el número 3"
elif [ "$numero" -eq 2 ];
        then
                echo "Has elegido el número 5"
elif [ "$numero" -eq 3 ];
        then
                echo "Has elegido el número 7"
else
                echo "Número incorrecto. Elige otro número"
fi
```
### 10: Evaluación de edad
```bash
#!/bin/bash
echo "Introduce tu edad:"
read edad
if (( edad < 18 ));
        then
                echo "Eres menor de edad"
elif (( edad >= 18 && edad <= 65 ));
        then
                echo "Eres adulto"
else
                echo "Eres mayor de 65 años"
fi
```
### 11: Contar líneas de un archivo
```bash
#!/bin/bash
echo "Introduce un nombre de archivo:"
read archivo

if [ -e "$archivo" ];
        then
                linea=$(wc -l  < "$archivo")
                echo "El archivo $archivo consta de $linea lineas"
else
                echo "El archivo $archivo no existe."
fi
```