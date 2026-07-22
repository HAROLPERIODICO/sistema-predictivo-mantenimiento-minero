
<div align=center>
  <img src="https://raw.githubusercontent.com/AhmedFathyDev/AhmedFathyDev/main/GitHub.gif" alt="GitHub Octocat Logo" height="100">
      <p>Loading</p>
</div>

<div align="center">
<h2>HPROYECTO DE GRADO</h2> <br>  ✨is a  _special_ ✨ repository because its ´README.md` <br> (this file) appears on your GitHub profile.
</div>

# Sistema Analítico Predictivo para la Optimización de Indicadores de Mantenimiento de Equipos Hidráulicos en la Minería del Caribe Colombiano 🚜📊

Proyecto de grado — Especialización en Analítica y Ciencia de Datos, Escuela de Ciencias Básicas, Tecnología e Ingeniería, UNAD (2026).

**Autor:** Harol Enrique Díaz Meléndez  
**Asesor:** Andrés Felipe Solis Pino  

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/HAROLPERIODICO/sistema-predictivo-mantenimiento-minero/blob/main/codigo/ProyectoGrado_final.ipynb)

---

## 📝 Descripción del Proyecto

Diseño e implementación de un sistema analítico predictivo, fundamentado en la metodología **CRISP-DM**, para el pronóstico de fallas y la optimización de la confiabilidad, disponibilidad y calidad del mantenimiento de equipos hidráulicos (21 palas hidráulicas Hitachi EX3600R) en una operación minera a cielo abierto del Caribe colombiano, bajo un contrato de Gestión de Activos a Riesgo Compartido (*MARC*).

El histórico del CMMS acumulaba 20.384 registros, de los cuales solo el 26.5% (5.404) correspondía a fallas mecánicas reales; el 73.5% restante era ruido operativo (mantenimientos preventivos, falsos reportes, daño operacional) mezclado sin distinción. El proyecto construye un pipeline ETL con modelo Entidad-Relación, entrena y compara seis algoritmos de clasificación bajo un esquema anti-fuga de datos, valida estadísticamente los resultados y despliega un tablero de decisión en Power BI.

* **Modelo Final:** `XGBoost` + `scale_pos_weight`
* **Rendimiento:** AUC de 0.807, Recall de 0.815 y Precision de 0.482 sobre datos nunca vistos en el set de prueba ciego Out-of-Time.
* **Impacto Financiero:** Ahorro neto estimado de **$766.500 USD** en seis meses de operación, ROI del **367.6%** y un periodo de recuperación (*payback*) menor a una semana.

---

## 📂 Estructura del Repositorio

```text
Proyecto_Grado/
├── codigo/
│   └── ProyectoGrado_final.ipynb      <-- Código fuente definitivo de Colab (Modelado y SHAP)
├── datos/
│   ├── DB_ProyectoGrado - DB_HISTORICO.csv  <-- Dataset crudo maestro original extraído del CMMS
│   └── datos_climaticos_guajira_DIARIO.csv <-- Histórico diario de control (Caché API Open-Meteo)
└── anexos_graficas/
    ├── 1_radiografia_flota_activa     
    ├── 2_tasa_retrabajos_flota_activa 
    └── 3_boxplot_auc_kfold 
    ├── 4_tabla_comparativa_modelos     
    ├── 5_curva_roc_comparativa 
    └── 6_importancia_variables 
    ├── 7_shap_summary     
    ├── 8_shap_waterfall_caso_critico 
    └── 9_curva_precision_recall
    ├── 10_shap_dependence_dur_evento    
    ├── 11_shap_dependence_fallas30d 
    └── 12_weibull_mtfs
    ├── 13_estacionalidad_clima    
    ├── 14_mapa_calor_clima_retrabajos
    └── 15_impacto_lluvia_retrabajos
    └── 16_reliability_diagram
    └── 17_shap_interaction_values

📝 Planteamiento del Problema y Justificación
En la minería a cielo abierto de la región semiárida del Caribe colombiano, mantener la disponibilidad de la flota de carga pesada es un pilar crítico para la rentabilidad de los contratos MARC. Sin embargo, el diagnóstico inicial reveló que la información del Sistema de Gestión de Mantenimiento Computarizado (CMMS) adolecía de una severa dispersión estructural.  
DOCX
+ 1

De los 20,384 registros históricos analizados, solo el 26.5% correspondían a fallas mecánicas reales. El 73.5% restante constituía ruido operativo (mantenimientos preventivos programados del sistema SEIS, órdenes duplicadas catalogadas como Falso Reporte y eventos de Daño Operacional). Esta fragmentación ocultaba los patrones de degradación física de los activos y forzaba al equipo de planeación a procesar datos de forma manual e ineficiente mediante hojas de cálculo convencionales.  
DOCX
+ 2

🚀 Innovación Clave: El Indicador MTFS (Mean Time Between Failure Service)
Para auditar de manera rigurosa la calidad de las intervenciones del taller, el proyecto introduce el indicador MTFS. Si el sistema hidráulico (SHI) de una pala reingresa por falla dentro de los cinco (5) días posteriores a una reparación, el evento se clasifica matemáticamente como Retrabajo (evidencia de que la falla original no se resolvió de raíz).  
DOCX
+ 1

Validación Estadística: La selección del umbral de 5 días fue validada mediante una prueba U de Mann-Whitney, demostrando que los equipos catalogados en el grupo de retrabajo exhibían un desgaste acumulado previo significativamente mayor (U=207,328; p<0.001; tamaño del efecto rank-biserial r=0.54).  
DOCX

Hallazgo Operativo: La aplicación del indicador expuso que 15 de las 21 palas activas superaban el umbral crítico de alerta del 15% de retrabajos, con unidades críticas (palas 60137 y 60138) registrando tasas alarmantes del 46.36% y 44.72%.  
DOCX

Análisis de Weibull: El ajuste por máxima verosimilitud de los intervalos de falla confirmó un parámetro de forma β=0.770 (η=15.54 días). Al ser β<1, se demostró científicamente que el régimen de falla dominante en la flota es de mortalidad infantil, lo cual es consistente con un problema crónico de calidad en las reparaciones.  
DOCX

🛠️ Desarrollo Técnico y Pipeline de Modelado
El motor analítico evalúa una matriz multivariable estructurada a partir de un Feature Engineering que captura 7 dimensiones operativas, destacando las variables de desgaste acumulado de corto y mediano plazo (Fallas_Ult_30d y Fallas_Ult_60d) construidas estrictamente a partir de registros pasados para evitar la contaminación temporal (data leakage).  
DOCX

Estrategia de Validación y Balanceo Óptimo
Se implementó una validación cruzada temporal mediante TimeSeriesSplit (k=5) y una partición cronológica Out-of-Time (Entrenamiento: 887 registros, Enero 2023 – Noviembre 2025; Prueba Ciego: 222 registros, Noviembre 2025 – Diciembre 2026).  
DOCX

Atendiendo la revisión experta, se compararon dos estrategias para el tratamiento del desbalance de clases (prevalencia de retrabajos del 34.5%) dentro del algoritmo ganador XGBoost:  
DOCX

Pipeline de Remuestreo Sintético: SMOTE-Tomek encapsulado internamente por fold para evitar la fuga de datos.  
DOCX

Manejo Nativo del Desbalance: Configuración del hiperparámetro scale_pos_weight.  
DOCX

Bajo el modelo de costos operativos del Contrato MARC, donde omitir una alerta (Falso Negativo ≈$15,000 USD) cuesta 30 veces más que una inspección preventiva adicional (Falso Positivo ≈$500 USD), la optimización del fbeta_score (β=2) determinó que el uso nativo de scale_pos_weight superaba a SMOTE-Tomek, consolidando el rendimiento definitivo en el set de prueba ciego.  
DOCX

Métricas de Evaluación Final (Set de Prueba Out-of-Time, n=222)
El rendimiento del modelo final de XGBoost se evaluó frente a otros algoritmos tradicionales y modelos formales de análisis de supervivencia (Cox y Random Survival Forest) mediante el índice de concordancia (c-index):  
DOCX

Métrica	Valor Obtenido	Significado Operativo
Accuracy	0.725	
Porcentaje global de aciertos del modelo.  
DOCX

Precision	0.524	
Eficiencia en la asignación de alertas preventivas en taller.  
DOCX

Recall (Sensibilidad)	0.815	
Capacidad para capturar y detener retrabajos reales antes de salir al tajo.  
DOCX

AUC ROC	0.807	
Robustez y poder de discriminación global del clasificador.  
DOCX

Significancia Estadística: Validada mediante pruebas pareadas de alta potencia (5x2cv paired t-test y Wilcoxon Signed-Rank Test) que confirmaron la robustez del modelo ganador frente a las alternativas evaluadas.  
DOCX

🌦️ Desmitificación de la Hipótesis Climática
Para investigar por qué la variable temporal "Mes" concentró el 36.50% de la importancia predictiva global del modelo, el pipeline integró una consulta automatizada en vivo a la Historical Weather API de Open-Meteo. Se analizaron 1,284 días de datos meteorológicos diarios reales (2023-2026) de la zona operativa en La Guajira (Lat: 11.16, Lon: -72.59).  
DOCX
+ 1

Las correlaciones de Pearson diarias y mensuales resultaron estadísticamente no significativas tanto para temperatura máxima (r=−0.102,p=0.753) como para precipitación y transiciones de choque atmosférico (r=0.029,p=0.436)[cite: 1]. Este hallazgo nulo demostró con rigor que la importancia de la estacionalidad mensual no es termodinámica sino organizativa e inherente al recurso humano, asociándose fuertemente a periodos críticos como Diciembre debido a dinámicas laborales (fatiga cognitiva por proximidad de festividades, ausentismo por vacaciones y relevos de personal)[cite: 1].

💰 Análisis Costo-Beneficio y Retorno de Inversión (ROI)
La implementación del sistema analítico predictivo se tradujo en un impacto financiero directo cuantificado mediante la Función de Pérdida Esperada del negocio[cite: 1]:

Costo Escenario Reactivo (Sin Modelo): $975,000 USD por fallas catastróficas imprevistas en tajo[cite: 1].

Costo Escenario Predictivo (Con XGBoost activo): $208,500 USD (derivado de Falsos Negativos residuales y el costo marginal de las inspecciones por Falsos Positivos)[cite: 1].

Ahorro Neto: $766.500 USD en solo seis meses de evaluación[cite: 1].

Métricas Financieras del Proyecto: ROI de 367.6% con un periodo de recuperación de la inversión (Payback) inferior a una semana frente al costo estimado de desarrollo[cite: 1].

⚙️ Metodología Aplicada (CRISP-DM)
Comprensión del negocio y de los datos: Levantamiento de requerimientos RF/RT, auditoría profunda del CMMS (20.384 registros iniciales)[cite: 1].

Preparación de datos: Pipeline ETL anti-fuga con filtros determinísticos en cascada y estructuración del modelo Entidad-Relación (MTBF, MTTR, MTFS)[cite: 1].

Modelado: Entrenamiento y optimización de hiperparámetros (GridSearchCV) comparando 6 algoritmos mediante TimeSeriesSplit[cite: 1].

Evaluación: Pruebas de hipótesis robustas, calibración de curvas isotónicas, interpretabilidad mediante valores SHAP individuales y Partial Dependence Plots[cite: 1].

Despliegue: Integración y validación funcional del prototipo interactivo en tableros de control de Power BI mediante pruebas UAT con usuarios finales[cite: 1].

📄 Licencia y Uso
Este repositorio aloja de forma exclusiva el trabajo de grado desarrollado por Harol Enrique Díaz Meléndez para optar al título de Especialista en Analítica y Ciencia de Datos en la Universidad Nacional Abierta y a Distancia (UNAD)[cite: 1]. Prohibida su reproducción con fines comerciales. Uso estrictamente académico e institucional.

