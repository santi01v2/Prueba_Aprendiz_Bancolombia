# Prueba_Aprendiz_Bancolombia
# Optimizador de Portafolios de Inversión

## Resumen del Proyecto

Este proyecto presenta un pipeline analítico completo diseñado para optimizar portafolios de inversión de clientes. La solución extrae datos de portafolios desde una base de datos PostgreSQL, los enriquece con datos de mercado obtenidos en tiempo real, y aplica el modelo de optimización de Markowitz para generar recomendaciones de rebalanceo que buscan maximizar el retorno ajustado por riesgo (Sharpe Ratio).

Este desarrollo fue realizado como parte de la prueba técnica para la posición de Aprendiz en la Gerencia Analítica de Inversiones.

---

## Objetivo del Negocio

El objetivo principal es proveer a los gerentes comerciales y asesores de inversión una herramienta cuantitativa que les permita:
1.  Visualizar la composición actual del portafolio de un cliente.
2.  Evaluar el rendimiento y riesgo esperado de dicho portafolio.
3.  Generar una recomendación de rebalanceo de activos basada en principios matemáticos robustos para mejorar la eficiencia del portafolio.

---

## Características Principales

* **Extracción de Datos:** Conexión a una base de datos PostgreSQL para obtener la composición de los portafolios de clientes.
* **Integración de Datos de Mercado:** Uso de la API de `yfinance` para obtener precios históricos de activos públicos.
* **Manejo de Datos Imperfectos:** Implementación de una estrategia de **proxies de mercado** para estimar el comportamiento de activos no públicos (bonos, fondos de renta fija, etc.). Es decir, aquellos activos no conocidos se modelan como activos de comportamiento parecido
* **Modelo de Optimización:** Aplicación de la Teoría Moderna de Portafolios (Markowitz) para encontrar la asignación óptima de activos que maximiza el Sharpe Ratio.
* **Generación de Reportes:** Creación de un reporte visual y autocontenido en formato HTML con `Plotly` que compara el portafolio actual del cliente con la recomendación del modelo.

---

## Stack Tecnológico

* **Lenguaje:** Python 3.12
* **Bases de Datos:** PostgreSQL
* **Análisis de Datos:** Pandas, NumPy
* **Datos de Mercado:** yfinance
* **Modelo de Optimización:** PyPortfolioOpt
* **Visualización:** Plotly
* **Conectividad DB:** SQLAlchemy, psycopg2

---

## Estructura del Proyecto

```
.
├── Conectar_bbdd.ipynb      # Notebook para configuración.
├── Limpieza_datos.ipynb # Notebook para la limpieza y extracción de BBDD.
├── Modelo_analitico.ipynb     # Notebook para optimizar y generar el reporte final y para obtener retornos y construir proxies.
├── README.md                       # Este archivo.
└── requirements.txt                # Dependencias del proyecto.
```

---

## Instalación y Ejecución

Este proyecto fue desarrollado en un entorno de GitHub Codespaces. Para ejecutarlo localmente:

1.  **Clonar el repositorio:**
    ```bash
    git clone cd ```

2.  **Crear un entorno virtual:**
    ```bash
    python -m venv env
    source env/bin/activate  # En Windows: env\Scripts\activate
    ```

3.  **Instalar las dependencias:**
    ```bash
    pip install -r requirements.txt
    ```
4.  **Configurar Variables de Entorno (Recomendado):**
    Para una conexión segura a la base de datos, se recomienda configurar las credenciales como variables de entorno en lugar de escribirlas directamente en el código.

5.  **Ejecutar el Pipeline:**
    Ejecutar los notebooks en orden para seguir el flujo lógico del proyecto:
    * `Conectar_bbdd.ipynb`: Carga los csv a AlwaysData.
    * `Limpieza_datos.ipynb`: Limpia los datos con funciones de SQL y carga los cambios a Alwaysdata
    * `Modelo_analitico.ipynb`: Construye el universo de mercado, incluyendo los retornos de los proxies y general el archivo de visualizacion

---

## Decisiones Clave de Modelado y Supuestos

El principal desafío analítico fue la falta de datos de mercado públicos para ciertos activos en los portafolios, como fondos de renta fija y bonos. Para superar esto, se implementó una **estrategia de proxies de mercado**, basada en los siguientes supuestos:

* **Renta Fija (Bonos, CDTs, Fondos de Liquidez):** Se utilizó el ETF **`AGG` (iShares Core U.S. Aggregate Bond ETF)** como proxy. Se asume que el comportamiento de este fondo es representativo del mercado de renta fija de bajo riesgo.
* **Renta Variable Local (Fondos Mixtos):** Se utilizó el índice **`ICOLCAP.CL`** como proxy para representar el comportamiento promedio del mercado de acciones colombiano.
* **Activos Internacionales No Identificados:** Para fondos o bonos internacionales sin ticker, se utilizó el índice **`^GSPC` (S&P 500)** como proxy general del mercado de acciones global.

El modelo de optimización fue configurado para **maximizar el Sharpe Ratio**, buscando el portafolio que ofrece el mayor retorno por cada unidad de riesgo asumida, asumiendo una tasa libre de riesgo del 2%.

---

# Vídeo 


---

## Futuras Mejoras

* **API de Tickers:** Implementar una API más robusta para convertir identificadores (CUSIP, ISIN) a tickers de mercado y reducir la dependencia de proxies.
* **Restricciones Avanzadas:** Añadir restricciones al optimizador, como límites de exposición por sector, por activo o por clase de activo.
* **Backtesting:** Construir un módulo de backtesting para evaluar el rendimiento histórico de la estrategia de rebalanceo sugerida por el modelo.

---
