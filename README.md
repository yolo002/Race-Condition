# Race Condition Solution

Este proyecto corrige una condición de carrera que ocurre al acceder a un recurso compartido (una variable en memoria) desde múltiples hilos. El ejemplo se basa en el código original del repositorio [race-condition](https://github.com/ValterMed/race-condition).

## Descripción del problema

En el ejemplo original, varios hilos acceden y modifican una variable compartida sin ninguna sincronización, lo que genera resultados inconsistentes debido a la **condición de carrera**.

### ¿Qué es una condición de carrera?
Es un problema que ocurre cuando varios hilos o procesos intentan modificar un recurso compartido de forma concurrente, provocando resultados inesperados.

## Solución implementada

Para resolver este problema, utilicé un **`threading.Lock`**. Este objeto asegura que solo un hilo pueda acceder al recurso compartido en un momento dado, evitando la condición de carrera.

### Código corregido

```python
import threading

# Recurso compartido
lock = threading.Lock()
shared_variable = 0

# Función que incrementa la variable compartida
def increment():
    global shared_variable
    with lock:  # Bloqueo para garantizar acceso exclusivo
        for _ in range(1000):
            shared_variable += 1

# Creación de múltiples hilos
threads = [threading.Thread(target=increment) for _ in range(10)]

# Inicio de los hilos
for t in threads:
    t.start()

# Esperar a que todos los hilos terminen
for t in threads:
    t.join()

# Resultado final
print(f"Final value of shared_variable: {shared_variable}")
