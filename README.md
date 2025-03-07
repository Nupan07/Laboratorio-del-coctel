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


## INICIO LABORATORIO 

Como primera instancia haremos la realizacion de la grabacion de los dos audios y los dos ruidos donde utilizaremos dos telefonos celulares para capturar las señales de audio en la siguiente aplicacion y configuracion de cada celular

![](https://github.com/Nupan07/Laboratorio-del-coctel/blob/main/Grabadoradevoz.jpg)


- Los micofonos se colocan a una distancia de 1 metro entre si de forma horizontal como se muestra en el siguiente diagrama
  
   ![](https://github.com/Nupan07/Laboratorio-del-coctel/blob/main/Microfonos.jpg)

  Donde ante esto iniciaremos la parte de programacion donde grabaremos 2 audios en 2 diferentes celulares y 2 ruidos los cuales se encontraran en .wav y los iniciaremos a llamar donde utilizaremos est fragmento

  # Listas de archivos de audio
archivos_voces = [r"C:\Users\valen\Downloads\Audio1.wav", 
                  r"C:\Users\valen\Downloads\Audio2.wav"]
archivos_ruido = [r"C:\Users\valen\Downloads\Ruidocel1.wav", 
                  r"C:\Users\valen\Downloads\Ruido2.wav"]

En el cual ante cargar esta señales procederemos a cargar y reproducir el audio en el cual usamos este fragmento 

def cargar_audio(ruta):
    señal, tasa_muestreo = librosa.load(ruta, sr=None)
    return señal, tasa_muestreo

def reproducir_audio(señal, tasa_muestreo):
    sd.play(señal, tasa_muestreo)
    sd.wait()

2. Iniciaremos con la graficacion de la onda y su espectro mediante este fragmento de codigo

   def mostrar_onda(señal, tasa_muestreo, titulo="Forma de onda"):
    tiempo = np.linspace(0, len(señal) / tasa_muestreo, num=len(señal))
    plt.figure(figsize=(10, 4))
    plt.plot(tiempo, señal, color='navy', lw=1.2)
    plt.xlabel('Tiempo (s)')
    plt.ylabel('Amplitud')
    plt.title(titulo)
    plt.grid(alpha=0.5)
    plt.show()

   El cual nos muestra la siguiente grafica

   ![](https://github.com/Nupan07/Laboratorio-del-coctel/blob/main/Se%C3%B1alM.png)

   Esta señal representa la mezcla de dos fuentes de audio antes de aplicar el proceso de separación con Análisis de Componentes Independientes (ICA). En los siguientes pasos del análisis, se intentará descomponer esta señal en sus fuentes originales para obtener las voces individuales

- Analisis espectral

   ![](https://github.com/Nupan07/Laboratorio-del-coctel/blob/main/Captura%20de%20pantalla%202025-03-06%20224827.png)

  3. Aplicaremos FastICA para la separacion de la señal
 
     def separar_fuentes(señal_mixta, num_fuentes=2):
    ica = FastICA(n_components=num_fuentes, random_state=0)
    señales_separadas = ica.fit_transform(señal_mixta.T).T
    return señales_separadas

Esta función toma una señal mixta (mezcla de varias fuentes de audio) y aplica Análisis de Componentes Independientes (ICA) para separar las fuentes originales.
 FastICA(n_components=num_fuentes) configura el modelo para extraer num_fuentes señales independientes.
 
✔ Transposición (.T) antes y después de fit_transform() es necesaria porque FastICA espera señales organizadas en columnas.
✔ La salida es un array donde cada fila representa una señal separada

![](https://github.com/Nupan07/Laboratorio-del-coctel/blob/main/CS1.png)

# Analisis espectral 

![](https://github.com/Nupan07/Laboratorio-del-coctel/blob/main/EspectroC1.png)

## AUDIO 2 (VOZ OLFRED)

![](https://github.com/Nupan07/Laboratorio-del-coctel/blob/main/Cs2.png)

**Analisis Espectral**

![](https://github.com/Nupan07/Laboratorio-del-coctel/blob/main/ES2.png)

Calculo SNR

def calcular_snr(señal, ruido):
    potencia_señal = np.sum(señal ** 2)
    potencia_ruido = np.sum(ruido ** 2)
    if potencia_ruido == 0:
        return float('inf')  
    snr = 10 * np.log10(potencia_señal / potencia_ruido)
    return snr

Donde nos arrojo los siguientes resultados :

![](https://github.com/Nupan07/Laboratorio-del-coctel/blob/main/SNR2.png)

## ANALISIS RESULTADOS 

**Relación Señal a Ruido (SNR)**

El cálculo del SNR permite evaluar la calidad de la separación de las fuentes de audio. Los resultados obtenidos son:

SNR para Audio 1: 16.18 dB
SNR para Audio 2: 19.93 dB

Estos valores indican que la separación de las señales fue efectiva, logrando recuperar las fuentes de manera clara. Un SNR más alto sugiere una mejor relación entre la señal útil y el ruido residual.

**Análisis Espectral**

Se generó un gráfico de análisis espectral donde se puede observar la distribución de las frecuencias de la señal separada. La mayor concentración de energía se encuentra en las bajas frecuencias, lo que es consistente con la presencia de componentes predominantes en esta región del espectro.

**Forma de Onda de la Señal Mixta**

En la representación temporal de la señal mixta, se observa una superposición de las fuentes originales. El análisis de esta forma de onda es clave para entender la complejidad de la separación.


