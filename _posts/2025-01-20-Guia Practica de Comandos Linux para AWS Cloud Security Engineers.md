---
layout: post
title: Guía Práctica de Comandos Linux para AWS Cloud Security Engineers
date: 2025-01-20 08:00:00 -0600
categories: linux seguridad aws
---

# Que debo saber hacer en linux como un cloud Security enginner?

Como AWS Cloud Security Engineer, tu día a día implica interactuar con sistemas Linux, ya sea configurando servidores, analizando logs o implementando medidas de seguridad. Esta guía te proporcionará una serie de escenarios prácticos y comandos esenciales que te ayudarán a desenvolverte con soltura en este entorno.


## Escenario 1: Identificar la Shell en uso y describir su función

**Objetivo:** Identificar la shell actual y entender su papel como intérprete de comandos.

Te has conectado a un servidor Linux remoto a través de SSH. Tu tarea es identificar la shell que estás utilizando y explicar brevemente su función.

**Pasos:**

1.  **Identificar la Shell:**

    *   **Comando:**

        ```bash
        echo $0
        ```
        o
        ```bash
        echo $SHELL
        ```

    *   **Salida Esperada:** `/bin/bash`, `/bin/zsh`, `/bin/sh`, etc. Esto indica que estás usando Bash, Zsh, o la shell Bourne, respectivamente.

2.  **Describir la Función de la Shell:**

    *   **Explicación:**

        > Estoy utilizando Bash (o la shell identificada). Bash es una shell de Unix y un intérprete de comandos. Su función es actuar como intermediario entre el usuario y el kernel de Linux. Recibe los comandos que ingreso, los interpreta y luego le pasa las instrucciones correspondientes al kernel para que las ejecute. Además, Bash permite la automatización de tareas a través de scripts, el manejo de variables de entorno, y proporciona características como el historial de comandos y la autocompletación, facilitando la interacción con el sistema.

## Escenario 2: Navegar por el sistema de archivos y manipular archivos y directorios

**Objetivo:** Practicar la navegación por el sistema de archivos y la creación, copia, movimiento y eliminación de archivos y directorios.


Como parte de una auditoría de seguridad, necesitas crear un directorio para almacenar logs, listar su contenido, crear un archivo de informe, copiarlo a otro directorio por seguridad y después eliminar el archivo original.

**Pasos:**

1.  **Navegar al directorio home del usuario actual:**

    *   **Comando:**

        ```bash
        cd ~
        ```

    *   **Explicación:** `cd` significa "change directory". El símbolo `~` es una abreviatura que representa el directorio home del usuario actual.

2.  **Crear un directorio llamado "logs_auditoria":**

    *   **Comando:**

        ```bash
        mkdir logs_auditoria
        ```

    *   **Explicación:** `mkdir` significa "make directory".

3.  **Navegar al directorio recién creado:**

    *   **Comando:**

        ```bash
        cd logs_auditoria
        ```

4.  **Mostrar el directorio de trabajo actual (para verificar):**

    *   **Comando:**

        ```bash
        pwd
        ```

    *   **Salida Esperada:** `/home/tu_usuario/logs_auditoria` (o algo similar, dependiendo de tu nombre de usuario y ruta al home).

5.  **Crear un archivo de texto llamado "informe.txt":**

    *   **Comando:**

        ```bash
        touch informe.txt
        ```

    *   **Explicación:** `touch` crea un archivo vacío. También puede usarse para actualizar la fecha de modificación de un archivo.

6.  **Listar los archivos en el directorio actual:**

    *   **Comando:**

        ```bash
        ls -l
        ```

    *   **Salida Esperada:** Verás `informe.txt` listado con sus permisos, propietario, tamaño y fecha de creación. `-l` muestra el listado en formato largo.

7.  **Crear otro directorio fuera de "logs_auditoria" para respaldar el informe:**

    *   **Comandos:**

        ```bash
        cd ..
        mkdir respaldo_informes
        ```

    *   **Explicación:** `cd ..` sube un nivel, saliendo de "logs_auditoria". `mkdir respaldo_informes` crea el nuevo directorio.

8.  **Copiar "informe.txt" al directorio de respaldo:**

    *   **Comando:**

        ```bash
        cp logs_auditoria/informe.txt respaldo_informes/
        ```

    *   **Explicación:** `cp` significa "copy". Estamos utilizando una ruta relativa. `logs_auditoria/informe.txt` es la ruta al archivo original desde la ubicación actual. `respaldo_informes/` es la ruta al directorio de destino.

9.  **Eliminar el archivo original "informe.txt" en "logs_auditoria":**

    *   **Comando:**

        ```bash
        rm logs_auditoria/informe.txt
        ```

    *   **Explicación:** `rm` significa "remove".

10. **Eliminar un directorio llamado "temp" que ya no es necesario:**
    *   **Comando:**

        ```bash
        rmdir temp
        ```
    *   **Explicación:** `rmdir` significa "remove directory". Solo funciona si el directorio está vacío.
    *   **En caso de que el directorio no esté vacío, se usa:**
        ```bash
        rm -r temp
        ```
        para eliminarlo con todo su contenido.

## Escenario 3: Explicar las implicaciones de seguridad de usar el usuario root y demostrar el uso correcto de sudo

**Objetivo:** Comprender los riesgos de seguridad asociados al uso del usuario root y aprender a utilizar `sudo` para realizar tareas administrativas de forma segura.

Necesitas instalar un software de monitorización de seguridad (por ejemplo, `fail2ban`) en un servidor, pero no se recomienda hacerlo directamente como root.

**Pasos:**

1.  **Explicar los Riesgos de Root:**

    *   **Explicación:**

        > El usuario root en Linux tiene control total sobre el sistema. Cualquier error cometido como root puede tener consecuencias graves, como el borrado accidental de archivos críticos del sistema o la ejecución de comandos maliciosos con privilegios ilimitados. Además, si un atacante obtiene acceso a la cuenta root, tendría control completo del servidor. Por estas razones, es una mala práctica de seguridad realizar tareas rutinarias como root.

2.  **Demostrar el Uso de Sudo:**

    *   **Comando para instalar `fail2ban` (en una distribución basada en Debian/Ubuntu):**

        ```bash
        sudo apt update
        sudo apt install fail2ban
        ```

    *   **Explicación:** `sudo` significa "superuser do". Permite a un usuario regular ejecutar comandos con los privilegios de root (u otro usuario) de forma temporal y controlada. Se te pedirá la contraseña de tu usuario (no la de root) para confirmar que tienes permiso para usar `sudo`. `apt update` actualiza la lista de paquetes disponibles, y `apt install fail2ban` instala el software.

    *   **Editar un archivo de configuración que requiere privilegios de root, por ejemplo, el archivo de configuración de SSH:**

        ```bash
        sudo nano /etc/ssh/sshd_config
        ```

    *   **Explicación:** `nano` es un editor de texto. `/etc/ssh/sshd_config` es la ruta al archivo de configuración de SSH. `sudo` eleva los privilegios para permitir la edición de este archivo protegido.

3.  **Verificar quién ha usado `sudo` recientemente (registro de auditoría):**

    *   **Comando:**

        ```bash
        sudo cat /var/log/auth.log | grep sudo
        ```

    *   **Explicación:** Este comando muestra las entradas del archivo de registro `/var/log/auth.log` que contienen la palabra "sudo". Aquí se registran los intentos exitosos y fallidos de usar `sudo`, incluyendo el usuario que lo ejecutó y el comando ejecutado. Esto es crucial para la auditoría de seguridad.
