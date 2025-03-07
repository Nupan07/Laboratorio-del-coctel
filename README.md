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
# 1)CARGA Y DISTRIBUCION DEL AUDIO 

