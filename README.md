# ranking-ciudades-espana

# 🏙️ Sistema de Ranking Urbano de España

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status](https://img.shields.io/badge/Status-Production-success.svg)]()

> **Sistema end-to-end de análisis multicriterio que evalúa 50 ciudades españolas en calidad de vida mediante datos oficiales, normalización estadística avanzada y personalización por perfiles de usuario.**

---

## 📋 Tabla de Contenidos

- [Descripción](#-descripción)
- [Características Principales](#-características-principales)
- [Metodología](#-metodología)
- [Datos y Fuentes](#-datos-y-fuentes)
- [Instalación y Uso](#-instalación-y-uso)
- [Estructura del Proyecto](#-estructura-del-proyecto)
- [Resultados](#-resultados)
- [Casos de Uso](#-casos-de-uso)
- [Roadmap](#-roadmap)
- [Contribuir](#-contribuir)
- [Licencia](#-licencia)
- [Contacto](#-contacto)

---

## 🎯 Descripción

Este proyecto resuelve el problema de **toma de decisiones sobre movilidad geográfica en España** mediante un sistema de scoring multicriterio que:

- Integra **13 variables** de 6 fuentes oficiales (INE, Ministerios)
- Analiza **50 ciudades** en 6 dimensiones de calidad de vida
- Genera **5 rankings personalizados** según perfil del usuario
- Exporta **10 datasets** listos para dashboards (Power BI, Tableau)

**Valor diferencial:** A diferencia de rankings genéricos, este sistema reconoce que "la mejor ciudad" varía según prioridades individuales (empleo vs seguridad vs educación).

---

## ✨ Características Principales

### 🔬 Rigor Metodológico

- **Normalización estadística avanzada** con inversión selectiva de variables negativas
- **Validación automática** de datos (nulos, duplicados, rangos)
- **Análisis de correlación** entre áreas temáticas
- **Documentación completa** de fórmulas y decisiones técnicas

### 🎭 Personalización por Perfiles

5 perfiles con ponderaciones diferenciadas:

| Perfil | Prioridades | Caso de Uso |
|--------|-------------|-------------|
| **General** | Equilibrado | Público amplio |
| **Jóvenes Profesionales** | Empleo (35%) + Transporte (15%) | 25-35 años buscando oportunidades |
| **Familias** | Educación (25%) + Seguridad (15%) | Familias con hijos |
| **Jubilados** | Sanidad (40%) + Seguridad (25%) | +65 años |
| **Nómadas Digitales** | Vivienda (25%) + Transporte (25%) | Trabajadores remotos |

### 📊 Outputs Listos para BI

- `dataset_completo_analisis.csv` (50×26 columnas) para análisis exploratorio
- `ranking_[perfil].csv` (5 archivos) para dashboards específicos
- `matriz_comparacion_top10.csv` para análisis de consistencia
- Resúmenes estadísticos y correlaciones

---

## 🧪 Metodología

### Pipeline de Procesamiento (7 Fases)

```
1. RECOPILACIÓN → Integración de 6 fuentes oficiales
2. VALIDACIÓN   → Limpieza y verificación de calidad
3. NORMALIZACIÓN → MinMaxScaler [0,1] con inversión selectiva
4. AGREGACIÓN   → Scores por área temática (promedio ponderado)
5. SCORING      → Cálculo de score total por perfil
6. RANKING      → Ordenamiento y asignación de posiciones
7. EXPORTACIÓN  → Generación de 10 archivos CSV
```

### Normalización Correcta de Variables

**Innovación clave:** Variables con direcciones opuestas requieren tratamiento diferenciado:

```python
# Variables POSITIVAS (↑ mejor): normalización directa
ingreso_norm = (ingreso - min) / (max - min)  # 36k€ → 1.0

# Variables NEGATIVAS (↓ mejor): normalización INVERTIDA
precio_norm = 1 - ((precio - min) / (max - min))  # 1,500€/m² → 1.0
```

### Cálculo de Score Total

```python
score_total = Σ(peso_área_i × score_área_i)

Donde:
  - peso_área_i: ponderación según perfil (ej: sanidad=40% para jubilados)
  - score_área_i: promedio de variables normalizadas del área
  - Restricción: Σ pesos = 1.0
```

---

## 📦 Datos y Fuentes

### Instituciones Oficiales

| Fuente | Variables | Actualización |
|--------|-----------|---------------|
| **INE** | Población, renta, empleo | Anual/Trimestral |
| **Ministerio del Interior** | Tasa de criminalidad | Anual |
| **Ministerio de Sanidad** | Centros salud, hospitales, médicos | Anual |
| **Ministerio de Educación** | Ratio alumnos/profesor, universidades | Anual |
| **MITMA** | Conectividad de transporte | Continua |
| **Idealista/Fotocasa** | Precio vivienda €/m² | Mensual |

### Variables Analizadas (13 total)

**Áreas Temáticas:**

1. **Vivienda** → Precio m² (↓)
2. **Mercado Laboral** → Tasa paro (↓), Renta anual (↑)
3. **Sanidad** → Centros salud (↑), Hospitales (↑), Médicos (↑)
4. **Educación** → Ratio alumnos/profesor (↓), Universidades (↑)
5. **Seguridad** → Tasa criminalidad (↓)
6. **Transporte** → Índice conectividad (↑)

**Leyenda:** ↑ = mayor es mejor | ↓ = menor es mejor

---

## 🚀 Instalación y Uso

### Requisitos

```bash
Python 3.8+
pandas>=2.0.0
numpy>=1.24.0
scikit-learn>=1.3.0
```

### Ejecución en Google Colab (Recomendado)

1. **Abrir notebook:** [Link al Colab]
2. **Ejecutar todas las celdas:** `Runtime > Run all`
3. **Descargar resultados:** Los CSV se generan automáticamente

### Ejecución Local

```bash
# Clonar repositorio
git clone https://github.com/tu-usuario/ranking-ciudades-espana.git
cd ranking-ciudades-espana

# Instalar dependencias
pip install -r requirements.txt

# Ejecutar análisis
python ranking_ciudades.py

# Outputs generados en carpeta /outputs
```

### Personalizar Pesos

Edita el diccionario `PERFILES` en el código:

```python
PERFILES = {
    'mi_perfil_custom': {
        'pesos': {
            'vivienda': 0.30,      # Ajusta según tus prioridades
            'mercado_laboral': 0.25,
            'sanidad': 0.15,
            'educacion': 0.10,
            'seguridad': 0.15,
            'transporte': 0.05
        }
    }
}
```

---

## 📁 Estructura del Proyecto

```
ranking-ciudades-espana/
│
├── README.md                          # Este archivo
├── requirements.txt                   # Dependencias Python
├── LICENSE                            # Licencia MIT
│
├── notebooks/
│   └── ranking_ciudades_completo.ipynb  # Notebook principal (Colab)
│
├── src/
│   ├── data_collection.py             # Módulo de recopilación
│   ├── data_validation.py             # Validación y limpieza
│   ├── scoring.py                     # Cálculo de scores
│   └── export.py                      # Generación de CSVs
│
├── data/
│   ├── raw/
│   │   └── dataset_raw_50_ciudades.csv
│   └── processed/
│       ├── dataset_completo_analisis.csv
│       ├── ranking_general.csv
│       ├── ranking_jovenes_profesionales.csv
│       ├── ranking_familias.csv
│       ├── ranking_jubilados.csv
│       ├── ranking_nomadas_digitales.csv
│       ├── matriz_comparacion_top10.csv
│       ├── resumen_estadistico_areas.csv
│       └── matriz_correlacion_areas.csv
│
├── docs/
│   ├── metodologia_completa.pdf       # Documentación técnica detallada
│   └── presentacion.pdf               # Slides de presentación
│
└── tests/
    ├── test_normalization.py
    └── test_scoring.py
```

---

## 📊 Resultados

### Top 10 Ranking General 2024

| Pos | Ciudad | Score | Fortalezas |
|-----|--------|-------|-----------|
| 1 | **Madrid** | 0.758 | Empleo, transporte, universidades |
| 2 | **Barcelona** | 0.742 | Renta alta, sanidad, conectividad |
| 3 | **Donostia-San Sebastián** | 0.721 | Seguridad, renta, sanidad |
| 4 | **Vitoria-Gasteiz** | 0.698 | Educación, seguridad |
| 5 | **Pamplona** | 0.692 | Bajo paro, seguridad |
| 6 | **Bilbao** | 0.685 | Renta, sanidad |
| 7 | **Valencia** | 0.668 | Equilibrio precio/servicios |
| 8 | **Zaragoza** | 0.652 | Vivienda asequible |
| 9 | **Santander** | 0.641 | Calidad de vida |
| 10 | **Salamanca** | 0.638 | Seguridad, educación |

### Insights Clave

🏆 **País Vasco domina:** 3 de las Top 6 ciudades (Donostia, Vitoria, Bilbao)
- Bajo paro (7-9% vs 12% nacional)
- Alta renta (30-36k€ vs 26k€ media)
- Criminalidad mínima (28-35/1000 vs >45 grandes urbes)

💰 **Ciudades medias ofrecen mejor ROI:**
- Valencia, Zaragoza, Málaga: 40-50% más baratas que Madrid/Barcelona
- Servicios comparables (sanidad, educación)
- Creciente conectividad post-pandemia

🔄 **Especialización geográfica:**
- **Norte:** Sanidad + Seguridad (Salamanca, Burgos, León)
- **Mediterráneo:** Clima + Lifestyle (Valencia, Málaga, Palma)
- **Inland:** Asequibilidad + Empleo estable (Zaragoza, Valladolid)

---

## 💼 Casos de Uso

### Para Individuos

- **Joven profesional (28 años):** ¿Dónde maximizar oportunidades laborales?
  → **Recomendación:** Barcelona (#1 jóvenes), Madrid (#2), Donostia (#3)

- **Familia con 2 hijos:** ¿Mejor educación y seguridad?
  → **Recomendación:** Vitoria (#1 familias), Pamplona (#2), Donostia (#3)

- **Jubilado (68 años):** ¿Mejor sanidad y tranquilidad?
  → **Recomendación:** Salamanca (#1 jubilados), Donostia (#2), Pamplona (#3)

### Para Empresas

- **Startups/Tech:** Identificar ciudades para expandir oficinas (bajo coste + talento)
- **Consultoras:** Estudios de viabilidad de apertura de sedes
- **Real Estate:** Análisis de mercados inmobiliarios infravalorados
- **Recursos Humanos:** Políticas de relocalización de empleados

### Para Investigación

- **Académica:** Benchmark para estudios de calidad de vida urbana
- **Política Pública:** Evaluar impacto de inversiones en infraestructura
- **Urbanismo:** Identificar patrones de desarrollo urbano equilibrado

---

## 🗺️ Roadmap

### ✅ Fase 1 (Completada - Q4 2024)
- [x] Sistema de ranking con 50 ciudades
- [x] 5 perfiles personalizados
- [x] Documentación completa

### 🚧 Fase 2 (Q1-Q2 2025)
- [ ] Expansión a 100 ciudades
- [ ] Añadir 7 variables: clima, calidad aire, cultura, espacios verdes
- [ ] Granularidad barrial (Madrid, Barcelona, Valencia)

### 📅 Fase 3 (Q3 2025)
- [ ] Dashboard interactivo (React + D3.js)
- [ ] API REST para consultas programáticas
- [ ] Validación con encuestas (500+ usuarios/perfil)

### 🔮 Fase 4 (Q4 2025 - 2026)
- [ ] Modelos predictivos: evolución de scores en el tiempo
- [ ] Integración con datos de empleo en tiempo real (LinkedIn, Indeed)
- [ ] App móvil (iOS/Android)

---

## 🤝 Contribuir

¡Las contribuciones son bienvenidas! Este proyecto es open-source.

### Cómo Contribuir

1. **Fork** el repositorio
2. **Crea una rama** para tu feature (`git checkout -b feature/nueva-variable`)
3. **Commit** tus cambios (`git commit -m 'Añadir variable: calidad del aire'`)
4. **Push** a la rama (`git push origin feature/nueva-variable`)
5. Abre un **Pull Request**

### Áreas de Contribución

- 📊 **Nuevas variables:** Datos adicionales de fuentes oficiales
- 🧪 **Testing:** Unit tests para validación de datos
- 📈 **Visualizaciones:** Gráficos y dashboards
- 🌍 **Internacionalización:** Adaptar a otros países
- 📝 **Documentación:** Mejorar claridad y ejemplos

### Guidelines

- Código en Python 3.8+
- Seguir PEP 8 para estilo
- Documentar funciones con docstrings
- Incluir tests para nuevas features

---

## 📄 Licencia

Este proyecto está bajo licencia **MIT License** - ver archivo [LICENSE](LICENSE) para detalles.

**Resumen:**
- ✅ Uso comercial permitido
- ✅ Modificación permitida
- ✅ Distribución permitida
- ✅ Uso privado permitido
- ⚠️ Sin garantía

---

## 📧 Contacto

**Marcos Borrillo**

- 💼 LinkedIn: [linkedin.com/in/tu-perfil](https://linkedin.com/in/tu-perfil)
- 🐙 GitHub: [@tu-usuario](https://github.com/tu-usuario)
- 📧 Email: tu.email@ejemplo.com

---

## 🙏 Agradecimientos

Datos proporcionados por:
- Instituto Nacional de Estadística (INE)
- Ministerio del Interior
- Ministerio de Sanidad
- Ministerio de Educación y Formación Profesional
- Ministerio de Transportes, Movilidad y Agenda Urbana
- Idealista y Fotocasa

---

## 📚 Referencias

- **Metodología MCDA:** Velasquez, M., & Hester, P. T. (2013). An analysis of multi-criteria decision making methods.
- **Mercer Quality of Living Survey:** [www.mercer.com](https://www.mercer.com)
- **The Economist Intelligence Unit - Global Liveability Index**

---

<div align="center">

**⭐ Si este proyecto te resultó útil, considera darle una estrella en GitHub**

[⬆ Volver arriba](#-sistema-de-ranking-urbano-de-españa)

</div>
