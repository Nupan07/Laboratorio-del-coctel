## DESCRIPCIÓN

En este laboratorio se aplicara el analisis de señales de voz en un entorno donde varias 
fuentes de sonido estan mezcaldas, como ocurre en alguna reunion o fiesta,la idea de este 
laboratorio es identificar y extraer la voz de un individuo a partir de grabaciones con multiples 
microfonos 

## PROBLEMA A RESOLVER 
En un evento tipo cóctel, se instalaron varios micrófonos para grabar conversaciones. Posteriormente, 
se requiere aislar la voz de una persona específica. Este problema es común en aplicaciones como el reconocimiento de voz, la cancelación de ruido y la mejora de la calidad de audio en sistemas de telecomunicaciones.

## OBJETIVOS

**1.** Aplicar técnicas de análisis en frecuencia para separar señales de voz mezcladas.

**2.** Comprender la problemática de la separación de fuentes sonoras en entornos con múltiples emisores de sonido.

**3.** Implementar y evaluar métodos de separación de señales,
como el Análisis de Componentes Independientes (ICA) o el ¨Beamforming¨.

**4.** Analizar el efecto de la ubicación de micrófonos y fuentes en la calidad de la separación de señales.

## ESTRUCTURA DEL LABORATORIO

**1.**  **Configuracion del sistema**
   
  - descargar micrifonos para tomar las señales requeridas 
  
  - Ubicar a las dos personas en diferentes lugares o posciones dentro 
    del lugar donde se tomara las señales  requeridas 
  
**2.** **Captura de la señal**
   
   - Cada persona pronuncia frases distintas mientras se graban las señales con los micrófonos.
   
   - se mide la relación señal/ruido (SNR) para evaluar la calidad de las grabaciones.
   
**4.**    **Procesamiento de la señal**

   - implementación de métodos de separación de fuentes (ICA, Beamforming, etc.).

**5.** **Evalucion de resultados**

   - Comparar la señal aislada con la original mediante métricas como la relación señal/ruido.
   
   - Analizar cómo la ubicación de los micrófonos afecta la separación de señales.
   
## MATERIALES NECESARIOS
1) Computador con acceso a internet.

2) Software de procesamiento de señales (En este caso Spyder).

3) Dos micrófonos.( o celulares )

4) NumPy (numpy) – Para el manejo de arreglos numéricos y cálculos matemáticos

5) Matplotlib (matplotlib.pyplot) – Para visualizar las señales en el dominio del tiempo y frecuencia.

6) SciPy (scipy.signal) – Para procesamiento de señales, filtrado y análisis en frecuencia.
   
7) import librosa  - Se usa para el análisis de señales de audio, como cargar archivos, calcular espectrogramas y extraer características.

8) import sounddevice as sd - Se usa para grabar y reproducir audio en tiempo real.

9) from sklearn.decomposition import FastICA  -  Se usa para realizar Análisis de Componentes Independientes (ICA), útil para separar fuentes de audio.

## RESULTADOS ESPERADOS

- Separación exitosa de las señales de voz, permitiendo escuchar por separado cada una de las voces grabadas.

- Análisis de la efectividad del método empleado y sugerencias de mejora.

## ICA(Análisis de Componentes Independientes )
Es una técnica de procesamiento de señales utilizada para separar señales mezcladas en sus componentes originales. En este laboratorio, el ICA se aplica al Problema del Cóctel, donde múltiples voces son grabadas simultáneamente por varios micrófonos, generando señales mezcladas.

El ICA asume que un conjunto de señales observadas (mezcladas) son combinaciones lineales de un conjunto de señales fuente desconocidas e independientes. Su objetivo es encontrar una transformación matemática que desmezcle las señales y recupere las fuentes originales.

Matemáticamente, si tenemos 𝑛 fuentes sonoras (voces) captadas por 𝑛 micrófonos, la relación entre ellas se puede modelar como:

            X = AS
Donde:

X es la matriz de señales observadas (las grabaciones de los micrófonos).

S es la matriz de las señales fuente originales (las voces individuales).

A es una matriz de mezclado desconocida (define cómo las voces se combinan en los micrófonos).

# ¿Cómo funciona el ICA ?

El Análisis de Componentes Independientes (ICA) se basa en dos suposiciones fundamentales:

1) Las fuentes de sonido son estadísticamente independientes entre sí. Esto significa que las señales de origen, como las voces de diferentes personas, no presentan una relación matemática directa.
   
2) Las señales originales poseen distribuciones no gaussianas. Un ejemplo de esto es la voz humana, la cual exhibe patrones específicos tanto en el dominio del tiempo como en el de la frecuencia.
   
Al aplicar ICA, es posible descomponer señales de audio mezcladas en un conjunto de señales independientes, con el objetivo de reconstruir las voces originales a partir de las grabaciones. Este proceso permite minimizar la interferencia entre las fuentes de sonido, mejorando la claridad de cada una de ellas.

## BEAMFORMING

Es una técnica avanzada de procesamiento de señales utilizada en sistemas de comunicación inalámbrica, radares y sistemas de audio. Su propósito principal es dirigir la señal en una dirección específica en lugar de transmitirla de manera omnidireccional. Esto mejora la calidad de la señal, reduce interferencias y optimiza el uso del ancho de banda.

# COMO FUNCIONA EL BEAMFORMING

El Beamforming se basa en el uso de una matriz de antenas o sensores dispuestos en una configuración específica. Cada antena dentro de esta matriz transmite la misma señal, pero con ajustes en la fase y amplitud de la onda. Estos ajustes se calculan de manera que:

*Las ondas se sumen constructivamente en la dirección deseada, aumentando la potencia de la señal.

*Las ondas se cancelen (interferencia destructiva) en direcciones no deseadas, reduciendo el ruido e interferencias.

## INICIO LABORATORIO 

Como primera instancia haremos la realizacion de la grabacion de los dos audios y los dos ruidos donde utilizaremos dos telefonos celulares para capturar las señales de audio en la siguiente aplicacion y configuracion de cada celular

![](https://github.com/Nupan07/Laboratorio-del-coctel/blob/main/Grabadoradevoz.jpg)


- Los micofonos se colocan a una distancia de 1 metro entre si de forma horizontal como se muestra en el siguiente diagrama
  
   ![](https://github.com/Nupan07/Laboratorio-del-coctel/blob/main/Microfonos.jpg)

  Donde ante esto iniciaremos la parte de programacion donde grabaremos 2 audios en 2 diferentes celulares y 2 ruidos los cuales se encontraran en .wav y los iniciaremos a llamar donde utilizaremos est fragmento

  # Listas de archivos de audio
# Listas de archivos de audio
archivos_voces = [r"C:\Users\USER\Music\señales\lab del coctel\Audio1.wav", 
                  r"C:\Users\USER\Music\señales\lab del coctel\Audio2.wav"]
archivos_ruido = [r"C:\Users\USER\Music\señales\lab del coctel\Ruidocel1.wav", 
                  r"C:\Users\USER\Music\señales\lab del coctel\Ruido2.wav"]
                  
# 1)CARGA , DISTRIBUCION y REPRODUCCION DEL AUDIO 

     def cargar_audio(ruta):
         señal, tasa_muestreo = librosa.load(ruta, sr=None)
         return señal, tasa_muestreo
         
Aca se especifica los datos de onda y la cantidad de muestras por segundo que se mostraran 

    def reproducir_audio(señal, tasa_muestreo):
         sd.play(señal / np.max(np.abs(señal)), tasa_muestreo)
         sd.wait()
Normaliza la señal (para evitar distorsión) y la reproduce. (sd.wait()) asegura que la reproducción termine antes de continuar.

# 2)MEZCLAR VOCES Y RUIDOES 

    def combinar_todos(archivos_audio, archivos_ruido):
       señales = [cargar_audio(archivo)[0] for archivo in archivos_audio + archivos_ruido]
       tamaño_min = min(map(len, señales))
       mezcla = sum(señal[:tamaño_min] for señal in señales) / len(señales)
       return np.column_stack((mezcla,)), cargar_audio(archivos_audio[0])[1]
       
 Carga todas las señales de voz y ruido,ajusta la longitud de todas las señales al tamaño más corto y crea una mezcla sumando y promediando las señales.

# 3)SEPARACION DE FUENTES DE ICA

    def separar_fuentes(señal):
       modelo_ica = FastICA(n_components=1, max_iter=1000, tol=0.0001)
       componentes = modelo_ica.fit_transform(señal)
       return componentes / np.max(np.abs(componentes))
       
Crea un modelo ICA con FastICA(n_components=1), lo que significa que intentará extraer una sola fuente de la mezcla de señales se ejecuta la separación con fit_transform(señal), que intenta encontrar la señal original sinruido y normaliza la salida para evitar distorsiones en la amplitud.

# 4)APLICACION DE BEAMFORMING

    def aplicar_beamforming(señal):
       U, S, Vt = svd(señal, full_matrices=False)
       señal_filtrada = np.outer(U[:, 0] * S[0], Vt[0, :])
       return señal_filtrada / np.max(np.abs(señal_filtrada))
       
Aplica SVD: La función svd(señal, full_matrices=False) descompone la señal en 3 matrices:

U → Representa las características de la señal.

S → Representa los valores singulares (importancia de cada componente).

Vt → Contiene información de las direcciones de los datos.

Reconstruye la señal usando solo el primer valor singular (S[0]), que generalmente contiene la información más importante y normaliza la señal para evitar cambios bruscos en el volumen

# 5)CALCULOS DEL SNR 

    def calcular_snr(señal, ruido):
       potencia_señal = np.sum(señal ** 2)
       potencia_ruido = np.sum(ruido ** 2)
       return 10 * np.log10(potencia_señal / potencia_ruido) if potencia_ruido > 0 else float('inf')
       
Calcula la potencia de la señal → np.sum(señal ** 2).

Calcula la potencia del ruido → np.sum(ruido ** 2).

Convierte la relación a decibeles (dB) usando 10 * np.log10(señal / ruido).

Se puede decir que las siguientes operaciones lograron hacer lo siguiente:

*ICA → Separa una voz de la mezcla.

*Beamforming (SVD) → Mejora la calidad de la señal.

*SNR → Mide la relación entre la señal y el ruido.

## GRAFICAS

# auidios y ruidos 

[![imagen-2025-03-07-163108816.png](https://i.postimg.cc/dttw3kPV/imagen-2025-03-07-163108816.png)](https://postimg.cc/dDXzS0Vb)

En esta primera imagen se puede observar el audio 1 con el ruido 1 y con el snr calculado 
El SNR calculado del Audio 1 con su Ruido: 16.35 dB, indica que la señal de audio es aproximadamente 43 veces más fuerte que el ruido.

[![imagen-2025-03-07-163823891.png](https://i.postimg.cc/ydQnTbd5/imagen-2025-03-07-163823891.png)](https://postimg.cc/ygRF1n0X)

El análisis espectral de la Señal 1 muestra la distribución de frecuencias en la señal de audio, evidenciando que la mayor parte de la energía se concentra en las bajas frecuencias, aproximadamente entre 0 y 1000 Hz, con picos significativos. A partir de esta región, la magnitud disminuye progresivamente, aunque persisten componentes de menor intensidad en frecuencias más altas, lo que podría ser indicativo de ruido. Esto sugiere que la señal tiene un fuerte contenido en bajas frecuencias, característico de la voz humana 

[![imagen-2025-03-07-164125472.png](https://i.postimg.cc/vHz77KNv/imagen-2025-03-07-164125472.png)](https://postimg.cc/BXj14ms8)

En esta segunda imagen se puede observar el audio 2 con el ruido 2 y con el snr calculado 
El SNR calculado del Audio 2 con su Ruido: 39.70 dB, indica que la señal es aproximadamente 9315 veces más fuerte que el ruido, lo que representa una calidad de audio muy buena.

[![imagen-2025-03-07-164454962.png](https://i.postimg.cc/KjYG6Rmj/imagen-2025-03-07-164454962.png)](https://postimg.cc/MMhJRZMJ)

El análisis espectral de la Señal 2 muestra que la mayor parte de la energía está concentrada en las bajas frecuencias, principalmente por debajo de los 1000 Hz, con picos bien definidos que indican componentes dominantes en esa región. A medida que la frecuencia aumenta, la magnitud disminuye, aunque aún se pueden observar pequeñas variaciones en el espectro que pueden corresponder a ruido o armónicos. Comparado con la Señal 1, 

# MEZCLA DE LOS AUDIOS Y RUIDOS 

[![imagen-2025-03-07-164717713.png](https://i.postimg.cc/8zb1zRsc/imagen-2025-03-07-164717713.png)](https://postimg.cc/9Dr3xqdH)

En esta grafica se puede observar las combinacion de los auidios con sus respecticos ruidos mostrandonos una señal mas grande y con mas ruido.

[![imagen-2025-03-07-164906357.png](https://i.postimg.cc/kgYpsGpw/imagen-2025-03-07-164906357.png)](https://postimg.cc/dhd6tsvT)

El análisis espectral de la mezcla de la combinación de la Señal 1 y la Señal 2, muestra una distribución de frecuencias similar a la de ambas señales individuales, con una concentración de energía predominante en las bajas frecuencias, especialmente por debajo de los 1000 Hz. Sin embargo, la magnitud en esta región parece ser una combinación de ambas señales, con picos más definidos que reflejan la superposición de sus componentes espectrales. En frecuencias más altas, la mezcla mantiene ciertas características de ruido, aunque con menor intensidad en comparación con la Señal 1, lo que sugiere que la mayor calidad fue la  de la Señal 2 .

# MEZCLA DE LOS AUDIOS Y RUIDOS CON UNA DESTACADA 

[![imagen-2025-03-07-165622058.png](https://i.postimg.cc/0jr6ptMV/imagen-2025-03-07-165622058.png)](https://postimg.cc/QKGxDJq7)

la señal que se destaca es la primera de la mezcla. Es decir, se multiplica por 1.5 la amplitud de la primera señal de audio en la mezcla para que tenga mayor presencia sobre la otra,
como se puede observar aumenta su amplitud entre la mezcla normal y la mezcla que se destaca.

[![imagen-2025-03-07-165852436.png](https://i.postimg.cc/YSkhX2LG/imagen-2025-03-07-165852436.png)](https://postimg.cc/75sHhkVw)

El análisis espectral de la mezcla, donde la Señal 1 ha sido aumentada , muestra un aumento en la magnitud de las frecuencias bajas, lo que indica que los componentes dominantes de la Señal 1 tienen mayor presencia en la combinación final. En comparación con el espectro de la mezcla anterior, se observa un refuerzo en los picos de energía por debajo de los 1000 Hz, lo que sugiere que la voz de la Señal 1 han sido resaltados o aumentado para que se noten mucho mejor en esta mezcla.

# APLICAR ICA Y SNR EN LAS MEZLCA 

[![imagen-2025-03-07-170153037.png](https://i.postimg.cc/N0gj6NZR/imagen-2025-03-07-170153037.png)](https://postimg.cc/jDFTKhxj)

Como se puede observar cuando se aplica el ICA en la mezcla su amplitud dismunuye ya que el  ICA encuentra componentes estadísticamente independientes en una mezcla de señales, y durante este proceso, los componentes extraídos pueden ser normalizados,reescalados o tambien  en otros casos se  puede atenuar componentes no deseados (como ruido o interferencias), lo que también puede dar la percepción de menor amplitud en la señal separada.con esta explicacion se puede inferir porque al aplicar el ica en la mezlca cambia su amplitud.

El  SNR después de ICA: 10.80 dB

[![imagen-2025-03-07-170919855.png](https://i.postimg.cc/L5fJCJDP/imagen-2025-03-07-170919855.png)](https://postimg.cc/q6JBgM9B)

El análisis espectral después de aplicar ICA muestra una distribución de frecuencias similar a la señal mezclada original, con la mayor parte de la energía concentrada en las bajas frecuencias (por debajo de 1000 Hz). Sin embargo, la magnitud general parece haberse reescalado, lo que puede ser una consecuencia del proceso de separación. Como el SNR sigue siendo 10.80 dB, el ICA no logró mejorar la relación señal a ruido.

# APLICAR BEAMFORMING Y SNR DE LA MEZCLA

[![imagen-2025-03-07-171100671.png](https://i.postimg.cc/dVJh6ty7/imagen-2025-03-07-171100671.png)](https://postimg.cc/fJgWTs5Z)

Como se puede observar en este grafica el Beamforming mantiene una amplitud muy parecida a la del ica en la mezcla esto quiere decir que su SNR no cambio demasiado , la razon de esto es porque Beamforming y ICA (Análisis de Componentes Independientes) comparten similitudes en la separación de señales, pero funcionan con principios diferentes.

SNR después de Beamforming: 10.81 dB

[![imagen-2025-03-07-171433328.png](https://i.postimg.cc/1Xxtq9S3/imagen-2025-03-07-171433328.png)](https://postimg.cc/LYT2dFhc)

El análisis espectral después de aplicar Beamforming  muestra una distribución de frecuencias similar a la señal mezclada original y la des´pues de aplicar el ICA, con la mayor parte de la energía concentrada en las bajas frecuencias (por debajo de 1000 Hz). El contenido de alta frecuencia sigue presente, pero en menor magnitud, lo que indica que el Beamforming no ha filtrado significativamente el ruido.

## ANALISIS DE DATOS 

El análisis de las señales revela que la Señal 1 tenía un SNR de 16.35 dB, indicando una calidad moderada con presencia de ruido, mientras que la Señal 2 tenía un SNR de 39.70 dB, lo que sugiere una señal mucho más limpia. La combinación de ambas señales generó un espectro con predominancia en frecuencias bajas y posibles interferencias, lo que pudo haber reducido el SNR global. Luego, se intentó destacar la Señal 1 en la mezcla, lo que aumentó su presencia en el espectro pero sin una mejora sustancial en la relación señal a ruido. 
