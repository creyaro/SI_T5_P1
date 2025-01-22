# Práctica 1: Creación y Detección de un Keylogger en Python

En esta práctica, aprenderemos cómo programar un keylogger básico en Python y a desarrollar un programa que lo detecte y elimine. 

## ✅ Entrega y evaluación

Deberás entregar el código generado, así como subirlo a GitHub y adjuntar el enlace del repositorio (llamado **SI_T5_P1**). Es **obligatorio** incluir un archivo README.md, donde se explique cómo funciona el keylogger, áreas de mejora, como usarlo, etc.

## 🖥️ Parte 1: Creación de un Keylogger Básico

Un keylogger es un programa que registra las teclas que pulsa el usuario. Aquí crearemos un keylogger sencillo que guarda las teclas pulsadas en un archivo de texto. 
Para ello, usaremos la biblioteca ``keyboard`` de Python, que es capaz de registrar los eventos del teclado. Para ello, sigue los siguientes pasos:

1. Crea un archivo llamado `keylogger.py`.

2. Instala la librería con el comando `pip install keyboard`.
3. Escribe el siguiente código para importar la librería:

   ```python
   import keyboard  
   
   # Lógica del keylogger
   ```

4. A continuación, desarrolla el resto del código por ti mismo. Recuerda que **deben registrarse las teclas que pulsa el usuario y guardarlas en un fichero**. Aunque puede parecer complicado, es tan sencillo como consultar la [documentación de la librería](https://github.com/boppreh/keyboard).


3. Prueba el programa:
   - Ejecuta el script en tu terminal o entorno de desarrollo:
     ```bash
     python keylogger.py
     ```
   - Abre cualquier aplicación y escribe algo.
   - Revisa el archivo donde se almacenan las teclas para verificar que se registraron las pulsaciones.


## 🧹 Parte 2: Creación de un Detector de Keyloggers

Ahora vamos a desarrollar un programa que busca keyloggers simples en el sistema, como el que creamos en la parte anterior, y elimina sus archivos de registro.

1. Crea un archivo llamado `keylogger_detector.py`.

2. Escribe el siguiente código:

   ```python
    import os
    import psutil

    # Nombre del archivo de log generado por el keylogger
    log_file = "keylog.txt"

    # Nombre del script que contiene el keylogger
    script_name = "keylogger.py" 

    # Función para encontrar y detener el proceso que ejecuta este script
    def detener_keylogger(script_name):
        try:
            # Recorre todos los procesos en ejecución
            for proc in psutil.process_iter(attrs=["pid", "name", "cmdline"]):
                if proc.info["cmdline"] and script_name in " ".join(proc.info["cmdline"]):
                    pid = proc.info["pid"]
                    print(f"Deteniendo proceso con PID {pid} ({script_name})...")
                    
                    # Termina el proceso
                    psutil.Process(pid).terminate()
                    print(f"Proceso {pid} detenido exitosamente.")
                    return
            print(f"No se encontró ningún proceso ejecutando {script_name}.")
        except Exception as e:
            print(f"Error al detener el proceso: {e}")

    # Función para eliminar el archivo de log
    def eliminar_archivo_log(log_file):
        try:
            if os.path.exists(log_file):
                os.remove(log_file)
                print(f"Archivo de log '{log_file}' eliminado exitosamente.")
            else:
                print(f"Archivo de log '{log_file}' no encontrado.")
        except Exception as e:
            print(f"Error al eliminar el archivo de log: {e}")

    detener_keylogger(script_name)
    eliminar_archivo_log(log_file)

   ```

3. Prueba el programa:
   - Ejecuta el script en el mismo directorio donde ejecutaste el keylogger:
     ```bash
     python keylogger_detector.py
     ```
   - Verifica si el archivo `keylog.txt` fue detectado y eliminado.