# Bitácora de Uso de Asistentes de Inteligencia Artificial

* **Herramienta:** Google Gemini / Gemini CLI.
* **Fecha:** Noviembre 2026.
* **Fases aplicadas:**
  1. **Diseño de pipeline geoespacial:** Generación y depuración del script de extracción e integración con Google Earth Engine y `geopandas` para la Región del Maule.
  2. **Procesamiento de imágenes satelitales:** Redacción de funciones de reproyección UTM (EPSG:32719) y fragmentación (*tiling*) en parches de 256x256 píxeles usando `rasterio`.
  3. **Estructuración del proyecto:** Redacción y formato en Markdown de la propuesta, bitácora y estado del arte según la pauta del curso.
* **Verificación:** Todo el código generado fue probado, ejecutado e inspeccionado visualmente en Google Colab. Las fuentes bibliográficas del estado del arte fueron verificadas manualmente contra las publicaciones originales.
