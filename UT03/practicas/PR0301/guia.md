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

### 7: Calificación de un examen

### 8: Comprobación del espacio en disco

### 9: Menú de opciones

### 10: Evaluación de edad

### 11: Contar líneas de un archivo
