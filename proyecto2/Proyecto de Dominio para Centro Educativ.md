# Proyecto de Dominio para Centro Educativo

 ## 1. Introducción

Este documento describe la estructura del dominio, la automatización del alta de alumnos, la organización de carpetas y las políticas de seguridad para un centro educativo que imparte los ciclos formativos de ASIR, SMR, DAM y DAW. La implementación de un dominio bien estructurado facilita la administración, mejora la seguridad y optimiza los recursos del centro educativo.

El objetivo de este proyecto es garantizar una gestión eficiente de los usuarios y equipos dentro de la red del centro educativo, asegurando que cada usuario tenga acceso a los recursos necesarios y que la seguridad esté controlada en todo momento.

## 2. Estructura del Dominio

### 2.1 Unidades Organizativas (OUs)

La estructura de Unidades Organizativas (OUs) permite organizar los usuarios y equipos de forma jerárquica, facilitando la asignación de permisos y políticas de seguridad. Se crearán las siguientes OUs dentro del dominio:

CentroEducativo (OU principal)

Usuarios

Alumnos

ASIR

Primero

Segundo

SMR

Primero

Segundo

DAM

Primero

Segundo

DAW

Primero

Segundo

Profesores

Equipos

Aulas (Cada aula con sus respectivos equipos)

La organización en OUs permite una mejor aplicación de políticas de seguridad, así como una administración más eficiente del dominio.

### 2.2 Grupos de Seguridad

Para simplificar la administración de permisos dentro del dominio, se crearán grupos de seguridad que agruparán a los usuarios según su rol y curso:

Grupos por curso (Ej.: ASIR_Primero, SMR_Segundo)

Grupo de profesores (Profesores)

Grupo de administradores (Administradores)

Cada grupo tendrá asignados permisos específicos sobre carpetas compartidas, políticas de seguridad y acceso a recursos dentro de la red del centro educativo.

## 3. Automatización del Alta de Alumnos

Para automatizar el alta de alumnos en el dominio, se utilizará un script en PowerShell que leerá el archivo CSV con los datos de los alumnos y creará automáticamente sus cuentas en Active Directory.

### 3.1 Creación de Usuarios en Active Directory

El script:

Lee el archivo CSV con los datos de los alumnos.

Genera un nombre de usuario basado en el nombre y apellidos del alumno.

Crea la cuenta en la Unidad Organizativa correspondiente.

Asigna una contraseña inicial y habilita la cuenta.

Agrega el usuario a su grupo de seguridad correspondiente.
```powershell
$usuarios = Import-Csv "C:\ruta\alumnos.csv"
foreach ($usuario in $usuarios) {
    $nombre = $usuario.Nombre
    $apellido1 = $usuario."Primer Apellido"
    $apellido2 = $usuario."Segundo Apellido"
    $ciclo = $usuario.Ciclo
    $curso = $usuario.Curso
    $username = ($nombre.Substring(0,1) + $apellido1 + $apellido2.Substring(0,1)).ToLower()
    
    $ouPath = "OU=$curso,OU=$ciclo,OU=Alumnos,OU=Usuarios,DC=centro,DC=local"
    New-ADUser -Name "$nombre $apellido1" -SamAccountName $username -UserPrincipalName "$username@centro.local" -Path $ouPath -AccountPassword (ConvertTo-SecureString "P@ssword123" -AsPlainText -Force) -Enabled $true
}
```
Este proceso garantiza una incorporación rápida y sin errores de los alumnos en el dominio.

## 4. Creación de Carpetas Personales y Compartidas

Cada alumno tendrá una carpeta personal para almacenar sus archivos y cada grupo de alumnos dispondrá de una carpeta compartida accesible para todos los miembros del grupo.

### 4.1 Automatización de la Creación de Carpetas

El siguiente script en PowerShell creará automáticamente las carpetas personales y compartidas, asignando los permisos correspondientes:
```powershell
$basePath = "\\servidor\Alumnos"
foreach ($usuario in $usuarios) {
    $folderPath = "$basePath\$($usuario.Ciclo)\$($usuario.Curso)\$($usuario.Nombre)$($usuario."Primer Apellido")"
    New-Item -Path $folderPath -ItemType Directory
    icacls $folderPath /grant "$($usuario.Nombre)$($usuario."Primer Apellido"):F"
}
```
Este sistema de almacenamiento permitirá a los alumnos acceder a sus archivos de manera ordenada y segura, evitando problemas de acceso no autorizado.

## 5. Políticas de Seguridad

Para garantizar la seguridad del sistema y evitar accesos no autorizados, se aplicarán políticas de grupo (GPO) que regulen el comportamiento de los usuarios dentro del dominio.

### 5.1 Políticas Implementadas

Restringir el acceso al cmd para alumnos. Evita el uso de comandos que puedan comprometer el sistema.

Impedir la instalación de software sin autorización. Protege contra la instalación de software malicioso.

Aplicar redirección de carpetas para datos de usuario. Permite que los archivos de los usuarios estén almacenados en el servidor.

Establecer políticas de contraseña seguras. Obliga a los usuarios a utilizar contraseñas fuertes.

Excluir a los profesores de ciertas restricciones. Los profesores necesitarán acceso a herramientas avanzadas.

## 6. Monitoreo y Mantenimiento

Para garantizar el buen funcionamiento del dominio, se implementará un sistema de monitoreo que incluirá:

Registro de eventos de seguridad para detectar accesos indebidos y posibles amenazas.

Scripts de mantenimiento que eliminan cuentas de alumnos inactivos.

Revisiones periódicas de políticas de seguridad para adaptarse a nuevas amenazas.

Supervisión del uso de almacenamiento para evitar la saturación del servidor.

El monitoreo activo permite detectar problemas de seguridad y corregirlos antes de que afecten el funcionamiento del sistema.

## 7. Conclusión

Este documento proporciona una estructura organizada y automatizada para la gestión del dominio en el centro educativo, asegurando eficiencia y seguridad en la administración de usuarios y recursos.

La implementación de estas medidas permitirá:

Un control más eficiente del acceso a los recursos.

Un sistema de almacenamiento seguro y estructurado.

Una automatización eficiente de la gestión de usuarios.

Una mejor protección contra amenazas de seguridad.

Con este proyecto, el centro educativo podrá garantizar una infraestructura tecnológica bien administrada, beneficiando tanto a alumnos como a profesores y asegurando un entorno de aprendizaje óptimo.

