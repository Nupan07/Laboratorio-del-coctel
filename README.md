Este Laboratorio está diseñado para procesar y analizar señales de audio, aplicando técnicas como la reducción de ruido y la separación de fuentes. Utilizamos herramientas para mejorar la calidad del audio y extraer voces individuales en un entorno de grabación.
Componentes Principales
Carga y Normalización de Audio:
Explicación: Cargamos los archivos de audio y los normalizamos para que tengan un rango de amplitud consistente. Esto facilita la comparación y el análisis de las señales.
Código:
import librosa
import numpy as np
def cargar_y_normalizar(archivos, ruta, frecuencia_muestreo):
    datos = []
    for archivo in archivos:
        señal, _ = librosa.load(os.path.join(ruta, archivo), sr=frecuencia_muestreo)
        señal_normalizada = normalizar_audio(señal)
        datos.append(señal_normalizada)
    return datos

def normalizar_audio(audio):
    return audio / np.max(np.abs(audio))
Visualización de Audio:
Explicación: Mostramos las señales de audio en el dominio del tiempo y la frecuencia para entender su estructura. Esto nos ayuda a identificar patrones y problemas en los datos.

Código:
import matplotlib.pyplot as plt
def visualizar_audio(audio, frecuencia_muestreo, titulo=''):
    tiempo = np.linspace(0, len(audio) / frecuencia_muestreo, len(audio))
    plt.figure(figsize=(14, 6))

    # Dominio del tiempo
    plt.subplot(2, 1, 1)
    plt.plot(tiempo, audio)
    plt.title(f'{titulo} - Dominio del Tiempo')
    plt.xlabel('Tiempo (s)')
    plt.ylabel('Amplitud')

    # Dominio de la frecuencia
    fft_audio = np.fft.fft(audio)
    frecuencias = np.fft.fftfreq(len(audio), 1 / frecuencia_muestreo)

    plt.subplot(2, 1, 2)
    plt.plot(frecuencias[:len(frecuencias)//2], np.abs(fft_audio[:len(fft_audio)//2]))
    plt.title(f'{titulo} - Dominio de la Frecuencia')
    plt.xlabel('Frecuencia (Hz)')
    plt.ylabel('Magnitud')

    plt.tight_layout()
    plt.show()
Filtrado Pasa Banda:
Explicación: Aplicamos un filtro pasa banda para eliminar frecuencias no deseadas, permitiendo solo el paso de las frecuencias dentro de un rango específico.
Código:
from scipy.signal import butter, lfilter

def filtro_pasa_banda(frecuencia_corte_inferior, frecuencia_corte_superior, frecuencia_muestreo, orden=6):
    nyquist = 0.5 * frecuencia_muestreo
    low = frecuencia_corte_inferior / nyquist
    high = frecuencia_corte_superior / nyquist
    b, a = butter(orden, [low, high], btype='band')
    return b, a

def aplicar_filtro_pasa_banda(datos_audio, frecuencia_corte_inferior, frecuencia_corte_superior, frecuencia_muestreo):
    b, a = filtro_pasa_banda(frecuencia_corte_inferior, frecuencia_corte_superior, frecuencia_muestreo)
    return [lfilter(b, a, audio) for audio in datos_audio]
Reducción de Ruido:
Explicación: Reducimos el ruido comparando las señales de audio con grabaciones de ruido para limpiar las señales. Utilizamos un umbral para eliminar las componentes de ruido.

Código:

def reducir_ruido(audio, ruido):
    if len(ruido) < len(audio):
        ruido = np.pad(ruido, (0, len(audio) - len(ruido)), 'constant')
    else:
        ruido = ruido[:len(audio)]

    fft_audio = np.fft.fft(audio)
    fft_ruido = np.fft.fft(ruido)
    abs_fft_ruido = np.abs(fft_ruido)

    umbral = np.percentile(abs_fft_ruido, 85)
    fft_limpio = np.where(abs_fft_ruido < umbral, fft_audio, fft_audio - fft_ruido)

    return np.real(np.fft.ifft(fft_limpio))
Cálculo del SNR (Relación Señal a Ruido):

Explicación: Calculamos la relación entre la potencia de la señal y la potencia del ruido para evaluar la calidad del audio. Un SNR alto indica una señal más clara.

Código:
def calcular_snr(señal, ruido):
    if len(ruido) < len(señal):
        ruido = np.pad(ruido, (0, len(señal) - len(ruido)), 'constant')
    else:
        ruido = ruido[:len(señal)]

    potencia_señal = np.sum(señal**2)
    potencia_ruido = np.sum(ruido**2)
    if potencia_ruido == 0:
        return float('inf')
    return 10 * np.log10(potencia_señal / potencia_ruido)
Separación de Voces con ICA (Análisis de Componentes Independientes):

Explicación: Utilizamos ICA para separar las voces de las grabaciones. ICA permite aislar las fuentes de sonido mezcladas, lo cual es crucial cuando se tienen múltiples voces o fuentes en una grabación.

Código:

from sklearn.decomposition import FastICA

def separar_voces(datos_audio, num_componentes):
    longitud_maxima = max(len(audio) for audio in datos_audio)
    audios_rellenos = [np.pad(audio, (0, longitud_maxima - len(audio)), 'constant') for audio in datos_audio]
    matriz_datos_audio = np.array(audios_rellenos).T

    ica = FastICA(n_components=num_componentes, random_state=42, max_iter=20000, tol=0.0001)
    fuentes = ica.fit_transform(matriz_datos_audio)
    return fuentes
Ejecución del Proceso
Cargar y Normalizar:

Cargamos y normalizamos los archivos de audio y ruido desde la ruta especificada.
Visualizar Audios:
Mostramos gráficos de las señales originales, filtradas y limpias en el dominio del tiempo y la frecuencia.
Aplicar Filtros y Reducción de Ruido:

Filtramos las señales y aplicamos reducción de ruido para mejorar la calidad de las grabaciones.
Calcular SNR:

Calculamos el SNR para evaluar la efectividad de la reducción de ruido.
Separar Voces:

Usamos ICA para separar las voces presentes en las grabaciones y visualizar las voces aisladas.
Guardar y Reproducir Voces:

Guardamos las voces separadas en archivos y las reproducimos para verificar la calidad de la separación.
Tecnologías Utilizadas
Librosa: Para la carga y normalización de audio.
NumPy: Para el procesamiento y análisis de datos.
Matplotlib: Para la visualización de las señales de audio.
Relación Señal a Ruido (SNR)
Qué es: La Relación Señal a Ruido (SNR, por sus siglas en inglés) es una medida utilizada para cuantificar la calidad de una señal en presencia de ruido. Se define como la relación entre la potencia de la señal útil y la potencia del ruido.

Cómo se mide: El SNR se expresa en decibelios (dB) 
Donde: SNR = 10 * log10(Potencia de la señal / Potencia del ruido)

Potencia de la señal es la energía de la señal deseada (en este caso, la señal de audio).
Potencia del ruido es la energía del ruido no deseado.
Por qué es importante: Un SNR alto indica que la señal es mucho más fuerte que el ruido, lo que significa que la calidad de la señal es buena. Un SNR bajo sugiere que el ruido está en niveles comparables a la señal, lo que puede hacer que la señal sea difícil de interpretar o procesar.

Aplicación en el proyecto: En el contexto del procesamiento de audio, calcular el SNR nos ayuda a evaluar qué tan bien se ha logrado limpiar una grabación de audio del ruido de fondo. Un buen SNR es crucial para asegurar que el audio procesado sea claro y útil para análisis o aplicaciones posteriores.

Análisis de Componentes Independientes (ICA)
Qué es: El Análisis de Componentes Independientes (ICA) es una técnica estadística utilizada para separar señales mezcladas en sus componentes independientes. ICA es especialmente útil en problemas donde múltiples señales están mezcladas en una única grabación y se desea recuperar las señales originales.

Cómo funciona: ICA asume que las señales observadas son una mezcla lineal de señales independientes. El objetivo de ICA es encontrar un conjunto de matrices que, cuando se aplican a las señales mezcladas, recuperen las señales originales independientes.

Principales conceptos de ICA:

Combinación Lineal: Las señales originales se combinan linealmente para formar las señales observadas.
Independencia: Las señales originales (fuentes) son estadísticamente independientes entre sí.
Por qué se usa: ICA se utiliza en una variedad de aplicaciones, como:
Separar fuentes de sonido en una grabación (por ejemplo, distinguir voces en una grabación de una fiesta).
Analizar datos en sistemas de EEG para identificar componentes cerebrales independientes.
