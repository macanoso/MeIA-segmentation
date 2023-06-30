  # MeIA reto: Segmentación semántica de vehículos en imágenes aéreas de drones usando Aprendizaje profundo


## Unet
A continuación, se muestra la propuesta de dos modelos de Redes Neuronales basadas en Unet, la primera U-Net para la segmentación de imágenes con la función de pérdida BinaryFocalCrossentropy. Contiene en su capa más grande en 1024 neuronas. Se utilizó una función de activación LeakyRelu para agregar no linealidad en el sistema y así fuese aún más robusto.

En la segunda red se muestra una red más amplia con un máximo de 2048 neuronas. Existe una desventaja al utilizar una red más grande, debido a que se está trabajando en un entorno colaborativo como lo es 'Google Colab' y 'Kaggle'; la limitación de recursos es importante considerar. Teniendo en cuenta esta limitación se opto por utilizar la primera opción.

¿Cómo evitar el Overfitting?

Se planteó utilizar diferentes optimizadores para mejorar el rendimiento del programa, aquí algunas de las opciones que se utilizaron:

1.   Adam: Adam es un optimizador popular que combina las ventajas del optimizador RMSprop.Es adecuado para una amplia gama de problemas y tiende a funcionar bien en la mayoría de los casos.

2.   RMSprop: RMSprop es un optimizador que ajusta las tasas de aprendizaje de forma adaptativa para cada parámetro. Es útil en problemas con gradientes dispersos.

3.  SGD: SGD es un optimizador básico que actualiza los parámetros en función del gradiente de la función de pérdida. Aunque es simple, a menudo se necesita un ajuste cuidadoso de la tasa de aprendizaje para obtener buenos resultados.

### Regularizadores y Dropout

La efectividad del dropout en comparación con los regularizadores como L1 o L2 puede variar dependiendo del problema, el modelo y los datos específicos. Sin embargo, hay algunas razones por las que el dropout puede funcionar mejor en ciertos casos:

*  Regularización estocástica: El dropout es una forma de regularización estocástica. Durante el entrenamiento, el dropout "apaga" aleatoriamente un conjunto de unidades (neuronas) en cada paso de entrenamiento, lo que hace que la red sea más robusta y evita que se vuelva demasiado dependiente de un subconjunto específico de características o conexiones. Esto puede ayudar a evitar el sobreajuste y mejorar la generalización del modelo.

*  Flexibilidad en la asignación de recursos: El dropout permite que las unidades de la red se adapten de manera más flexible y dinámica durante el entrenamiento, ya que no pueden confiar en la presencia constante de otras unidades. Esto puede ayudar a evitar la coadaptación de características y promover una representación más diversa y generalizable.

*   Menor necesidad de ajuste de hiperparámetros: El dropout tiene un hiperparámetro principal, la tasa de dropout, que controla la probabilidad de "apagar" una unidad. Sin embargo, en general, el dropout es menos sensible a la elección precisa de la tasa de dropout en comparación con los regularizadores L1 o L2. Esto puede hacer que el ajuste de hiperparámetros sea más sencillo y menos propenso a errores.


### Funciones de activación

Se realizaron pruebas con la funcion de activación de leaky relu, la cual al no ser 0 siempre que la entrada sea un número negativo nos ayuda a aprender más fácilmente los patrones presentes en el dataset, reduciendo considerablemente el overfitting, teniendo en cuenta que para los datos de validación se obtuvo 95% de accuracy y 0.225 de error y en datos de entrenamiento teniendo un accuracy de 97%, lo que nos deja con una diferencia de 2% entre datos de entrenamiento y error, lo cual se acerca bastante al ideal. En contraposición, usando la función de activación relu (la que fue entregada en el código previo a modificaciones), se obtuvieron datos similares en entrenamiento, sin embargo, en datos de validación se tiene un accuracy de 85%, viendo así que la función de activación leaky relu incrementa el rendimiento de la red sustancialmente.



### Aumento de datos

Otra técnica para enfrentar el desbalanceo de datos es el aumento de datos. Consiste en aplicar transformaciones a las imágenes (y máscaras, si se requiere), para obtener un conjunto más amplio de datos. Para ello, se optó por realizar transformaciones geométricas a las imágenes, las cuales consisten en rotaciones y reflexiones. En total se logró ampliar cuatro veces el conjunto de datos.
Se aplicaron las transformaciones geométricas previo a la implementación de la red, mediante el uso de la librería “Albumentations”. Esto, si bien, requiere de más recursos de almacenamiento, nos ayuda a ver las transformaciones de las imágenes con sus máscaras correspondientes.

### callbacks

Mediante los callbacks se implementó la verificación de mejora en las épocas y el guardado de la que presentaba el mejor desempeño. Esto permite guardar el modelo ante una falla en el entrenamiento o usarlo para hacer predicciones. También, se guardó la mejor época según el 'accuracy' de los datos de validación.

### Resultados

En la Tabla I, se especifican las direcciones de los resultados con y sin aumentar el dataset. Se dejaran estos dos archivos para tener un punto de comparación. Además, cada Notebook tiene una variación en el parámetro **threshold** porque es sensible al visualizar los resultados.

*Tabla I. Resultados según el tamaño del dataset.*

|Tipo|pdf|Notebook|
|---|---|---|
|Sin aumento| [main_without_Data_Augmentation.pdf](./main_without_Data_Augmentation.pdf) |[main_without_Data_Augmentation.ipynb](./main_without_Data_Augmentation.ipynb) |
|Con aumento| [main_with_Data_Augmentation.pdf](./main_with_Data_Augmentation.pdf) | [main_with_Data_Augmentation.ipynb](./main_with_Data_Augmentation.ipynb) |

### Conclusiones

* Al entrenar una red neuronal muchos hiperparametros entran en juego, de los cuales se pueden hacer diferentes combinaciones como lo pueden ser funciones de activación, funciones de pérdida, optimizadores entre otros; además en el preprocesamiento de datos existen diferentes técnicas de visión por computadora que pudiera dar mejores resultados; probar todo es una tarea complicada pero puede ser factible en un entorno diferente donde se cuente con más recursos y tiempo.
  
* Una de las limitaciones principales de este proyecto es el hardware necesario para resolver este tipo de problemas, al ser un trabajo de segmentación, el procesamiento de imágenes es muy demandante; las herramientas que se utilizaron fueron virtuales como lo son “Google Colab” y “Kaggle”; esto fue un problema ya que al utilizar cuentas gratuitas se tenían recursos limitados. El problema principal fue en la etapa de entrenamiento ya que se limitó a un red estándar la cual no se pudo extender por dichas limitaciones.
  
* Las transformaciones empleadas para el aumento de datos fueron geométricas. Hizo falta explorar más tipos de transformaciones que optimizaran la calidad de los datos. No se incluyeron transformaciones espectrales, por lo que explorarlas en el aumento de datos podría dar resultados significativos. Las transformaciones se implementaron antes de procesar las imágenes, por lo que imágenes transformadas pudieron entrar como parte del conjunto de test. Esto pudo influir en las predicciones del modelo. Otra limitante es el número de transformaciones, pues multiplica la cantidad de datos a procesar, por lo que los entrenamientos toman más tiempo y resulta complicado evaluar diferentes resultados con diferentes transformaciones en un tiempo limitado.

* En la visualización de las predicciones se estuvieron probando diferentes valores de threshold. En el caso del modelo sin datos aumentados, el threshold con valor de 0.2 mostró una mejor definición en la detección de vehículos, disminuyendo el ruido de los alrededores. En el modelo con datos aumentados, un threshold de 0.1 ayudó a definir mejor los vehículos detectados.

## Bibliografía

* Brownlee, J. (2019, enero 30). How to Choose Loss Functions When Training Deep Learning Neural Networks. MachineLearningMastery.Com. https://machinelearningmastery.com/how-to-choose-loss-functions-when-training-deep-learning-neural-networks/

* Buragohain, A. (s. f.). Albumentations Documentation—Using Albumentations with Tensorflow. Recuperado 29 de junio de 2023, de https://albumentations.ai/docs/examples/tensorflow-example/

* Cano, M. (2023). MeIA-segmentation [Jupyter Notebook]. https://github.com/macanoso/MeIA-segmentation (Obra original publicada en 2023)

* Nayak, R. (2022, abril 26). Focal Loss: A better alternative for Cross-Entropy. Medium. https://towardsdatascience.com/focal-loss-a-better-alternative-for-cross-entropy-1d073d92d075


* Pérez Gutiérrez, F. (2020). Detección de vehículos en circulación mediante visión artificial y redes neuronales convolucionales [MSc]. Universidad Complutense de Madrid.

* Team, K. (s. f.). Keras documentation: Base Callback class. Recuperado 29 de junio de 2023, de https://keras.io/api/callbacks/base_callback/

* Tf.keras.losses.BinaryFocalCrossentropy | TensorFlow v2.12.0. (s. f.). TensorFlow. Recuperado 29 de junio de 2023, de https://www.tensorflow.org/api_docs/python/tf/keras/losses/BinaryFocalCrossentropy

* Yathish, V. (2022, agosto 4). Loss Functions and Their Use In Neural Networks. Medium. https://towardsdatascience.com/loss-functions-and-their-use-in-neural-networks-a470e703f1e9

## Herramientas

* Phind: AI search engine. (s. f.). Recuperado 29 de junio de 2023, de https://www.phind.com/

* Project Jupyter. (s. f.). Recuperado 29 de junio de 2023, de https://jupyter.org

## Equipo de trabajo

* Juan Sebastian Zapata Echeverri
* Johnny Rubio Pecasso
* Cristhian Daniel    Guevara Cano
* Valentina Narváez Gaitán
* Claudia Vanessa Arevalo Rodriguez
* Santiago Moya Duran
* Esteban Ladino Fajardo


## Agradecimientos

A todo el equipo de [MeIA](https://www.taller-tic.redmacro.unam.mx/MeIA/).
