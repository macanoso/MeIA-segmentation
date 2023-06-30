# Reto Segmentación de Vehículos con Drones Equipo1
En este repositorio se sube el notebook que corresponde al desafío de segmentación de vehículos con drones que realizó el equipo 1.

## Integrantes:

Padilla Luis Edgar Abidán

Reyes Castro Luis Manuel

Sánchez García Carlos Alberto

Daza Liñan Raul Andres

Ruiz Ortega Francisco Javier


Parra Guardia Nelson Fernando

Alvarez Ricardo Santiago

Benavide Lopez Javier Alexis

Sandoval Casallas Carlos Santiago

Martinez Serrano Alejandro

## Resumen

El objetivo de este trabajo es mejorar las métricas obtenidas en el modelo “semantic msegmentation-hagdavs” descargable de: https://www.kaggle.com/datasets/mateocanosolis/hagdavs?resource=download

Basados en una arquitectura de UNET, se realizó una modificación en el bloque de upsample en la capa de Convolution Up a Upsampling2D.
Se realizó un aumento de la data de entrenamiento (imágenes y máscaras), utilizando las herramientas de keras, para ello se utilizó:

    • RandomCrop, con rellenos de un valor contante de 0
    • Normalización
    • RadomFlip (horizontal y vertical)
    • RandomTranslation (veritical y horizontal)
    • Random Constrate
    
En una visualización previa se había utilizado el RandomRotation de 10º, sin embargo, se creaban unas deformaciones en las imágenes que a juicio del equipo introduciría errores en el modelo, por lo que se optó por eliminar.

La función de perdida utilizada fue: "binary_crossentropy". La métrica utilizada fue IoU.

Se configuró el entrenamiento a 30 épocas, sin embargo, se adaptó en la compilación un callbacks con un earlystopper, para detener el entrenamiento si la perdida en el set de validación no mejoraba en 5 épocas seguidas.

Se obtuvo una accuracy de 0.8846, una pérdida de 11.13 y un IoU de 0.4836. El entrenamiento se detuvo a las 17 épocas, cuando no mejoró más la perdida. Cabe destacar que previamente se había corrido un modelo sin data augmentation, con resultados de accuracy: 0.9771, loss: 0.0708 y IoU de 0.6779 (imagen 1)

![image](https://github.com/Edgar-Padilla/RetoSegmentacionEquipo1/assets/97701913/d2ad9b87-7d5b-4ecb-9cec-045668f6105b)
Imagen 1

## Conclusiones.

Se debe mejorar el uso del aumento de datos, ya que se tuvo métricas por debajo de las originales obtenidas sin data augmentation. Es probable que existan transformaciones que introduzca ruido a las imágenes.

Se puede probar una perdida distinta para ver si se mejora el desempeño de la red, y por ende de las métricas.

Se puede diseñar un código que filtre las máscaras con un bajo porcentaje de pixeles de la clase minoritaria, de esta manera se elimina formas de autos que no corresponden, por efecto de haber cortado la imagen principal.
