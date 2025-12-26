# 🚀 Guía de Inicio Rápido

**Tiempo estimado:** 5 minutos

## Opción 1: Google Colab (Más Fácil)

### Paso 1: Abrir Notebook

Haz clic aquí: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1ts45Fs-x4eUVJf9lbREOkz9k_fl9vITE)

### Paso 2: Ejecutar
```
Runtime > Run all (Ctrl+F9)
```

### Paso 3: Descargar Resultados

Los archivos CSV se generan automáticamente. Para descargarlos todos:
```python
from google.colab import files
import os

archivos = [f for f in os.listdir() if f.endswith('.csv')]
for archivo in archivos:
    files.download(archivo)
```

---

## Opción 2: Ejecución Local

### Requisitos
- Python 3.8+
- pip

### Instalación
```bash
# 1. Clonar repositorio
git clone https://github.com/mborrillo/ranking-ciudades-espana.git
cd ranking-ciudades-espana

# 2. Instalar dependencias
pip install -r requirements.txt

# 3. Ejecutar notebook
jupyter notebook notebooks/ranking_ciudades_completo.ipynb
```

---

## 🔍 Explorar Resultados

### En Python
```python
import pandas as pd

# Cargar ranking general
df = pd.read_csv('ranking_general.csv', index_col='posicion')

# Top 10 ciudades
print(df.head(10)[['ciudad', 'score_total']])

# Buscar una ciudad específica
madrid = df[df['ciudad'] == 'Madrid']
print(f"Madrid - Posición: #{madrid.index[0]}, Score: {madrid['score_total'].values[0]:.3f}")

# Comparar dos ciudades
ciudades = ['Madrid', 'Barcelona']
comparacion = df[df['ciudad'].isin(ciudades)][
    ['ciudad', 'score_vivienda', 'score_mercado_laboral', 'score_sanidad']
]
print(comparacion)
```

---

## 🎯 Casos de Uso Rápidos

### "¿Cuál es la mejor ciudad para mí?"

1. **Identifica tu perfil:**
   - 25-35 años buscando empleo → `ranking_jovenes_profesionales.csv`
   - Familia con hijos → `ranking_familias.csv`
   - Jubilado → `ranking_jubilados.csv`
   - Trabajador remoto → `ranking_nomadas_digitales.csv`

2. Abre el CSV correspondiente
3. Revisa el Top 10

### "Comparar Madrid vs Barcelona"
```python
df = pd.read_csv('dataset_completo_analisis.csv')
ciudades = df[df['ciudad'].isin(['Madrid', 'Barcelona'])]

print("\n=== VIVIENDA ===")
print(ciudades[['ciudad', 'precio_vivienda_m2', 'score_vivienda']])

print("\n=== EMPLEO ===")
print(ciudades[['ciudad', 'tasa_paro', 'ingreso_medio_anual']])
```

---

## ❓ FAQ

**P: ¿Los datos están actualizados?**
R: Sí, datos de 2023-2024. Algunos con 1-2 años de retraso (ej: renta media).

**P: ¿Puedo modificar los pesos?**
R: Sí, edita el diccionario `PERFILES` en el notebook.

**P: ¿Puedo usar esto comercialmente?**
R: Sí, licencia MIT lo permite.

---

**Siguiente:** Ver [README.md](README.md) para documentación completa
