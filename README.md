# Trabajo-Jimmy

# Pipeline de integración de datos de movilidad — Medellín

Este proyecto ejecuta un pipeline de adquisición, limpieza, validación e integración de tres fuentes de datos relacionadas con movilidad y seguridad vial en Medellín:

1. Semáforos de Medellín, obtenidos desde la API SIGMOV de la Alcaldía.
2. Siniestros viales ocurridos entre 2016 y 2025, filtrados para Medellín y por exceso de velocidad.
3. Datos simulados de un radar FMCW de 24 GHz asociado a 50 semáforos.

El resultado final relaciona los siniestros con la red semáforo-radar mediante una unión espacial por proximidad.

## Cómo ejecutarlo

1. Abrir el archivo `Copia_de_Entregable_4-9.ipynb` en Jupyter Notebook, Google Colab o VS Code.
2. Ejecutar todas las celdas en orden, desde la primera hasta la última.
3. El pipeline descarga automáticamente los datos necesarios y crea las carpetas de salida si no existen.

No requiere configuración adicional ni archivos de entrada previos.

## Requisitos

- Python 3
- pandas
- geopandas
- shapely
- requests
- matplotlib

Instalación sugerida:

```bash
pip install pandas geopandas shapely requests matplotlib
```

## Flujo del pipeline

1. **Adquisición de semáforos:** consulta la API de la Alcaldía de Medellín; si no está disponible, usa un archivo de respaldo.
2. **Carga de siniestros:** descarga la base nacional de siniestros viales 2016–2025.
3. **Generación del radar simulado:** crea 150.000 registros de velocidad para 50 semáforos, con frecuencia de muestreo de 100 ms.
4. **Limpieza y validación:** elimina registros sin identificador, estandariza categorías, valida coordenadas, fechas, duplicados y rangos de velocidad.
5. **Integración semáforo-radar:** une ambos conjuntos por `numidentificador`.
6. **Integración espacial:** relaciona los siniestros por exceso de velocidad con el semáforo-radar más cercano.
7. **Salidas y gráficas:** genera los archivos finales y visualizaciones de siniestros y excesos de velocidad por semáforo.

## Archivos generados

| Carpeta | Archivo | Contenido |
|---|---|---|
| `data/raw/` | `semaforos.csv` | Datos crudos de semáforos |
| `data/raw/` | `siniestros.csv` | Base original de siniestros |
| `data/raw/` | `radarsimulado.csv` | Datos simulados del radar |
| `data/processed/` | `semaforoslimpio.csv` | Semáforos depurados |
| `data/processed/` | `siniestroslimpio.csv` | Siniestros de Medellín por exceso de velocidad |
| `data/processed/` | `radarlimpio.csv` | Datos del radar validados |
| `data/integrated/` | `semaforosensor.csv` | Unión semáforo-radar |
| `data/integrated/` | `datostotalesmovilidad.csv` | Resultado final integrado |

## Resultado esperado

Al finalizar la ejecución, el pipeline debe generar:

- 275 semáforos válidos.
- 3.088 siniestros en Medellín asociados a exceso de velocidad.
- 150.000 registros de radar simulado.
- Una tabla final integrada con los siniestros vinculados espacialmente a los semáforos con sensor.

## Notas

- La semilla aleatoria está fijada en `SEED = 42`, por lo que la simulación del radar es reproducible.
- El pipeline valida automáticamente la calidad de los datos mediante `assert`.
- Si la API de semáforos no responde, el código utiliza el respaldo descargado desde GitHub.
