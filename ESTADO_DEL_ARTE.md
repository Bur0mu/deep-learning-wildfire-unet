# Estado del Arte - Predicción de Riesgo de Incendios Forestales

| Referencia (APA) | Tarea | Dataset | Familia de Arquitectura | Métrica y Protocolo | Resultado Reportado |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Bjånes et al. (2021)** | Susceptibilidad a incendios forestales | 15 factores satelitales (NDVI, topografía, clima), parches 25x25 | Ensamble de 2 CNN vs XGBoost y SVM | AUC-ROC, Accuracy, Sensibilidad y Especificidad | AUC 0.953 (Ensamble); 0.902 (Mejor CNN) |
| **Shen et al. (2023)** | Evaluación de peligro de incendio (FireRisk) | 91,872 imágenes aéreas NAIP (EE.UU.) etiquetadas en 7 clases | Supervisadas y autosupervisadas (MAE con ImageNet-1k) | Accuracy multiclase | 65.29% (MAE) |
| **Kondylatos et al. (2023)** | Peligro de incendio a corto plazo (Mesogeos) | Datacube del Mediterráneo (2006-2022), 25,722 muestras | LSTM, Transformer y Graph Transformer Networks (GTN) | Precision, Recall, F1, AUPRC. Train 2006-19, Val 2020, Test 2021-22 | AUPRC de 0.853 a 0.858 |

### Cierre y Elección de Arquitectura
Usaré una red U-Net de segmentación semántica con un backbone preentrenado (ResNet34 con pesos de ImageNet), adaptada a nuestros tensores multicanal (NDVI y NDMI). Adopto la métrica **Mean Intersection over Union (mIoU)** y el **Coeficiente Dice** para evaluar el desempeño, complementado con latencia de inferencia por tile (milisegundos) para medir la eficiencia. Nuestro escenario se diferencia por trabajar con parches Sentinel-2 a 20 m de resolución en la Región del Maule, con un límite acotado de cómputo (2 h en T4) y evaluación estricta sin fuga espacial entre tiles adyacentes.

### Referencias
* Bjånes, A., De La Fuente, R., & Mena, P. (2021). A deep learning ensemble model for wildfire susceptibility mapping. *Ecological Informatics*, 65, 101397.
* Kondylatos, S., Prapas, I., Camps-Valls, G., & Papoutsis, I. (2023). Mesogeos: A multi-purpose dataset for data-driven wildfire modeling in the Mediterranean. *Advances in Neural Information Processing Systems*, 36, 50661-50676.
* Shen, S., Seneviratne, S., Wanyan, X., & Kirley, M. (2023). FireRisk: A remote sensing dataset for fire risk assessment with benchmarks using supervised and self-supervised learning. En *2023 IEEE DICTA* (pp. 189-196).
