# Limpieza y Manipulación de Datos con Pandas

## Objetivo del Proyecto

Este proyecto, correspondiente al **Sprint 10 de Python**, se centra en el uso avanzado de la librería **Pandas** para la manipulación, limpieza y preparación de datos.

El objetivo es simular un ciclo de vida completo de análisis de datos, comenzando con un fichero de datos "sucio" (el archivo `sprint10.xlsx`). A través de los ejercicios, se realizan tareas de limpieza de cabeceras, transformación de tipos de datos, "feature engineering" (creación de nuevas columnas a partir de otras), y análisis agregado mediante `groupby` y `pivot_table`.

Finalmente, el proyecto explora funciones más abstractas, como la automatización de la visualización de datos y la implementación de un algoritmo heurístico para resolver un problema de optimización de rutas.

---

## Estructura del Repositorio

| Archivo / Carpeta | Descripción |
| --- | --- |
| `sprint_10.ipynb` | Notebook de Jupyter con todos los enunciados, código y ejecuciones. |
| `sprint10.xlsx` | Datos fuente (brutos) sobre los trabajadores para los Niveles 1 y 2. |
| `matriu_distancies.xlsx` | Datos fuente (matriz) para el ejercicio de optimización de rutas (Nivel 3). |
| `dades_GrupA.xlsx` | Ejemplo de fichero exportado automáticamente (Nivel 2). |
| `dades_GrupB.xlsx` | Ejemplo de fichero exportado automáticamente (Nivel 2). |
| `dades_GrupC.xlsx` | Ejemplo de fichero exportado automáticamente (Nivel 2). |
| `dades_GrupD.xlsx` | Ejemplo de fichero exportado automáticamente (Nivel 2). |
| `resumen_grupos_profesionales.xlsx` | Fichero de resumen exportado con estadísticas por grupo (Nivel 2). |
| `titanic_age.png` | Ejemplo de gráfico autogenerado por la función del Nivel 3. |
| `titanic_deck.png` | Ejemplo de gráfico autogenerado por la función del Nivel 3. |
| `README.md` | Este documento. |

---

## Nivel 1 — Limpieza y Preparación de Datos

### 1. Importación y Ordenación Inicial

**Objetivo:**  
Importar correctamente un fichero `.xlsx` que tiene cabeceras y filas de título innecesarias. Una vez cargado, ordenar el DataFrame por `País d'origen` y `Ciutat`, y comprobar la unicidad de la columna `DNI`.

---

### 2. Transformación de Columnas

**Objetivo:**  
Realizar un seguido de tareas de limpieza y "feature engineering" para mejorar la legibilidad y utilidad del DataFrame.

**Características:**
* **Concatenación:** Crear una nueva columna `Nombre_Completo`.
* **Condicional:** Crear una columna `Nacido en España` ('Si'/'No') basada en `País d'origen`.
* **Índice:** Establecer la columna `DNI` como índice del DataFrame.
* **Renombrar:** Cambiar el nombre de las columnas de fecha (Ej: `Dia de Naixement` por `Dia`).
* **Reemplazar:** Sustituir los valores de `Gènere` (H, D, A, NC) por etiquetas claras (`Home`, `Dona`, `Altres`) y gestionar los valores nulos (`np.nan`).

---

### 3. Combinación de Columnas con .apply()

**Objetivo:**  
Consolidar dos columnas (`Fills` y `No Fills`), que contenían `NaN` y `1.0`, en una sola columna categórica (`Fills`) con los valores 'Sí' o 'No'. Esto se resuelve mediante una función personalizada y el método `.apply(axis=1)`.

---

### 4. Tabla Resumen Agregada

**Objetivo:**  
Limpiar la columna `Salari mensual` (eliminando " €" y convirtiéndola a `float`) y crear una tabla resumen.

**Características:**
* Agrupa los datos por `Gènere`.
* Calcula el salario **medio**, **mediano**, **mínimo** y **máximo** para cada grupo.
* Ordena la tabla final por el salario medio.

---

### 5. Tabla Dinámica (Pivot Table)

**Objetivo:**  
Crear una tabla dinámica (`pivot_table`) que muestre el salario medio por `Gènere` (como índice/filas) y `País d'origen` (como columnas).

**Extra:**
* Se añaden los márgenes (`margins=True`) para calcular las medias totales por fila y columna.
* Se aplica formato condicional (`.style.background_gradient`) para crear un "heatmap" que resalta visualmente los salarios más altos.

---

### 6. Gestión de Fechas (Datetime)

**Objetivo:**  
Trabajar con datos de tipo fecha y hora.

**Características:**
* Crear una nueva columna `Data_naixement` en formato `datetime` a partir de las columnas `Dia`, `Mes` y `Any`.
* Definir una función que, dada una fecha de nacimiento, calcule la edad actual de la persona.
* Aplicar esta función para crear la columna `Edat`.

---

## Nivel 2 — Integración y Exportación de Dadas

### 1. Fusión de DataFrames (Merge)

**Objetivo:**  
Actualizar los salarios del DataFrame principal basándose en un nuevo DataFrame de incrementos.

**Pasos:**
1.  Crear un DataFrame (`df_increment`) con los porcentajes de incremento para cada `Grup Professional`.
2.  Limpiar los datos de porcentaje (Ej: "3,5%" -> 0.035).
3.  Fusionar (`pd.merge`) los dos DataFrames.
4.  Actualizar la columna `Salari mensual` aplicando el incremento correspondiente.

---

### 2. Exportación Automatizada con Bucles

**Objetivo:**  
Automatizar la exportación de datos filtrados utilizando un bucle `for`.

**Resultados:**
* **4 Archivos de Datos:** Se genera un fichero `.xlsx` independiente por cada `Grup Professional` (ej: `dades_GrupA.xlsx`, `dades_GrupB.xlsx`...).
* **1 Archivo Resumen:** Se exporta un 5º fichero (`resumen_grupos_profesionales.xlsx`) que contiene el **recuento de trabajadores**, el **sueldo medio** y la **edad mediana** para cada grupo.

---

## Nivel 3 — Funciones Abstractes y Algoritmos

### 1. Función de Visualización Automática

**Objetivo:**  
Crear una función genérica y reutilizable (`generar_graficos_auto`) que acepte *cualquier* DataFrame como parámetro y genere (y guarde) automàticamente un gráfico adecuado para cada columna.

**Lógica de la Función:**
* **Si la columna es Numérica:** Genera un **histograma**.
* **Si la columna es Categórica (o tiene pocos valores únicos):** Genera un **gráfico de barras** con el recuento de frecuencias.
* **Si la columna es Datetime:** Genera un **gráfico de barras** contando los valores por año.
* La función se prueba con el dataset "titanic" de Seaborn.

---

### 2. Algoritmo de Optimización de Ruta (Nearest-Neighbor)

**Objetivo:**  
Implementar una solución heurística para el "Problema del Viajante" (Traveling Salesperson Problem), buscando una ruta corta para visitar las principales ciudades de España.

**Pasos:**
1.  **Carga y Limpieza:** Cargar el fichero `matriu_distancies.xlsx`, establecer el índice correctamente, y eliminar las ciudades no accesibles por carretera (islas).
2.  **Algoritmo:** Se crea una función que implementa el algoritmo del "vecino más cercano" (Nearest-Neighbor):
    * Comenzar en una ciudad de origen.
    * Ir siempre a la ciudad más cercana que todavía no haya sido visitada.
    * Repetir hasta visitar todas las ciudades.
3.  **Resultado:** La función devuelve la lista con el orden de la ruta y la distancia total recorrida.

**Extra:**
* Se ejecuta el algoritmo en un bucle, probando cada ciudad como punto de inicio, para determinar qué ciudad de origen ofrece la ruta total más corta según esta heurística.

---

## Conclusión

Este Sprint representa un salto significativo, pasando de los fundamentos de Python a la aplicación práctica de **Pandas** para el análisis de datos del mundo real. Se ha cubierto el ciclo completo: carga, limpieza exhaustiva, transformación, agregación, fusión, y exportación de datos, culminando con la creación de funciones complejas y abstractas que automatizan tareas de análisis y visualización.
