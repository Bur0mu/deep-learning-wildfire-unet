# Propuesta Proyecto Final - Deep Learning 2026-II

## 1. Identificación
* **Título:** Predicción de Zonas con Riesgo de Incendio Forestal en la Región del Maule Mediante U-Net y Datos Espectrales Sentinel-2
* **Nombre:** Marcelo Pablo Faúndez Báez
* **Repositorio:** (https://github.com/Bur0mu/deep-learning-wildfire-unet)

## 2. Problema y para quién
Mejora la toma de decisiones preventivas de personal de CONAF, Senapred y brigadas de emergencia al priorizar patrullajes y cortafuegos en la Región del Maule. Actualmente, el riesgo se evalúa mediante promedios meteorológicos globales por comuna sin resolución espacial continua, lo que dificulta detectar zonas de vegetación seca/estresada en microsectores específicos.

## 3. Tarea
* **Entrada:** Tiles/parches de imágenes satelitales multispectrales de 256x256 píxeles con canales de índices espectrales (NDVI y NDMI) a 20m de resolución.
* **Salida:** Máscara binaria/mapa de calor indicando la probabilidad de susceptibilidad de incendio por píxel.
* **Tipo de tarea:** Segmentación semántica.

## 4. Corpus
Imágenes Sentinel-2 procesadas mediante Google Earth Engine sobre la Región del Maule, combinadas con datos tabulares de estrés ambiental y puntos de calor térmicos de la NASA (FIRMS). El volumen de datos procesados (tiles GeoTIFF de 256x256) no supera los 450 MB. Se aplica una partición de 70% entrenamiento, 15% validación y 15% prueba, separando espacialmente los tiles para evitar la fuga de datos por contigüidad. Los datos se procesan y descargan mediante scripts en el código.

## 5. Escalera de modelos
1. **Baseline ML Clásico:** Random Forest Classifier entrenado a nivel de píxel utilizando los valores de NDVI, NDMI y estadísticas locales de textura.
2. **Baseline Deep Learning (desde cero):** Red U-Net estándar básica de 4 niveles entrenada desde cero (sin pesos preentrenados).
3. **Modelo Propuesto (Transfer Learning):** U-Net con backbone preentrenado (ResNet34) usando la librería `segmentation-models-pytorch`. Se congelan las primeras capas convolucionales del encoder y se realiza fine-tuning del decoder para adaptar los índices espectrales a las máscaras de riesgo.

## 6. Métricas
* **Efectividad:** Mean Intersection over Union (mIoU) y Coeficiente Dice por clase, para evaluar la precisión en la segmentación del área en riesgo.
* **Eficiencia:** Latencia de inferencia por tile (milisegundos por parche de 256x256) y tiempo de entrenamiento total.

## 7. Visualización interactiva de resultados
Interfaz interactiva desarrollada en **Streamlit** que permite seleccionar un tile o zona del Maule y desplegar la predicción visual de la U-Net (mapa de riesgo) junto a los canales NDVI y NDMI. Para la defensa presencial, se ejecutará de manera 100% local en laptop cargando el modelo exportado en PyTorch (`.pth`).

## 8. Recursos de cómputo y de IA
* **Equipo local:** CPU AMD, GPU NVIDIA GeForce GTX 1660 SUPER, 16 GB RAM.
* **Plan Colab:** Colab Gratis (GPU NVIDIA Tesla T4 con 15 GB VRAM). El tiempo estimado de entrenamiento estará acotado a menos de 40 minutos continuos.
* **Asistentes de IA:** Google Gemini CLI / Gemini como asistente de arquitectura, refactorización de código y generación de máscaras de datos (declarado para bitácora de IA).

## 9. Riesgo principal y plan B
* **Riesgo principal:** Desbalance extremo de clases entre píxeles de vegetación/suelo normal y píxeles etiquetados con alto riesgo o historial de incendio.
* **Plan B:** Emplear una función de pérdida combinada Focal Loss + Dice Loss y ajustar el umbral de decisión (*thresholding*) en la salida sigmoide del modelo.
