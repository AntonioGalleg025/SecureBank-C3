# SecureBank - Segunda Entrega

**Asignatura: Sistemas Operativos (2024–2025)**  
**Universidad Francisco de Vitoria**  
**Proyecto: Simulador de un Sistema Bancario Concurrente Avanzado**
**Autores: Antonio Gallego Ortiz, Adrián Zamorano Valero, Hugo Guarido Domínguez, Manuel Luna Jiménez, José Polanco Andrés**

## Descripción General

Esta práctica extiende el sistema SecureBank desarrollado en la primera entrega, incorporando mecanismos avanzados de gestión de memoria, sistema de ficheros y entrada/salida. El sistema simula múltiples usuarios accediendo y operando sobre un banco de forma concurrente y segura, utilizando los recursos del sistema operativo Linux y el lenguaje de programación C.

La arquitectura del sistema se divide en tres módulos principales:

- **memoria.c**: Gestión de la memoria compartida.
- **ficheros.c**: Registro de transacciones en un sistema de ficheros por usuario.
- **entrada_salida.c**: Manejo de entrada/salida con buffer circular y sincronización asíncrona con disco.

## Funcionalidades Desarrolladas

### 1. Gestión de Memoria Compartida
- Se implementa un segmento de memoria compartida donde se almacena una tabla global de cuentas bancarias.
- Los procesos hijos (usuarios) acceden directamente a esta tabla para consultar y modificar información.
- Se usa `pthread_mutex_t` para sincronizar el acceso a la memoria y evitar condiciones de carrera.

### 2. Sistema de Ficheros
- Cada usuario tiene un directorio único en `/transacciones/<número_de_cuenta>/` donde se registra su actividad.
- Cada transacción se registra cronológicamente en un archivo `transacciones.log`.
- Se emplean semáforos para evitar conflictos al escribir en los ficheros compartidos.

### 3. Entrada/Salida con Buffer
- Se implementa un buffer circular que almacena temporalmente las operaciones realizadas por los usuarios.
- Un hilo dedicado monitoriza este buffer y vuelca las operaciones en el archivo `cuentas.dat` de forma asíncrona.
- La lectura de saldo se realiza en tiempo real desde la memoria, sincronizándose con disco cuando es necesario.

### 4. Planificación de Operaciones (Extra)
- Se ha desarrollado un planificador de operaciones por prioridad:
  - Alta: Retiros y transferencias.
  - Media: Consultas de saldo.
  - Baja: Generación de reportes.
- Las operaciones se ordenan mediante `qsort()` y son procesadas por un hilo que ejecuta en base a dicha prioridad.

## Cómo Ejecutar la Práctica

Para ejecutar correctamente la práctica, se deben seguir los siguientes pasos desde la raíz del proyecto:

1. Accede al directorio `src`:
   ```bash
   cd src
   ```

2. Concede permisos de ejecución al script de inicio:
   ```bash
   chmod 777 Launch.sh
   ```

3. Ejecuta el sistema con privilegios de superusuario:
   ```bash
   sudo ./Launch.sh
   ```

Esto iniciará el proceso principal del sistema, lanzará los procesos de usuario, cargará las cuentas en memoria compartida y comenzará a registrar transacciones en los logs correspondientes.
