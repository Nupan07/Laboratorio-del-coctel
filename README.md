## DESCRIPCIÓN

En este laboratorio se aplicara el analisis de señales de voz en un entorno donde varias 
fuentes de sonido estan mezcaldas, como ocurre en alguna reunion o fiesta,la idea de este 
laboratorio es identificar y extraer la voz de un individuo a partir de grabaciones con multiples 
microfonos 

## PROBLEMA A RESOLVER 
En un evento tipo cóctel, se instalaron varios micrófonos para grabar conversaciones. Posteriormente, 
se requiere aislar la voz de una persona específica. Este problema es común en aplicaciones como el reconocimiento de voz, la cancelación de ruido y la mejora de la calidad de audio en sistemas de telecomunicaciones.

## OBJETIVOS

1)Aplicar técnicas de análisis en frecuencia para separar señales de voz mezcladas.
2)Comprender la problemática de la separación de fuentes sonoras en entornos con múltiples emisores de sonido.
3)Implementar y evaluar métodos de separación de señales,
como el Análisis de Componentes Independientes (ICA) o el ¨Beamforming¨.
4)Analizar el efecto de la ubicación de micrófonos y fuentes en la calidad de la separación de señales.

## ESTRUCTURA DEL LABORATORIO

1) # CONFIGURACION DEL SISTEMA
  *descargar micrifonos para tomar las señales requeridas 
  *Ubicar a las dos personas en diferentes lugares o posciones dentro 
  del lugar donde se tomara las señales  requeridas 
  
2) # CAPTURA DE LA SEÑAL
   *Cada persona pronuncia frases distintas mientras se graban las señales con los micrófonos.
   *se mide la relación señal/ruido (SNR) para evaluar la calidad de las grabaciones.
   
4) # PROCESAMIENTO DE SEÑALES
   *implementación de métodos de separación de fuentes (ICA, Beamforming, etc.).

5) # EVALUACION DE RESULTADOS
   *Comparar la señal aislada con la original mediante métricas como la relación señal/ruido.
   *Analizar cómo la ubicación de los micrófonos afecta la separación de señales.
   
## Materiales Necesarios
1)Computador con acceso a internet.
2)Software de procesamiento de señales (Python).
3)Dos micrófonos.
4)NumPy (numpy) – Para el manejo de arreglos numéricos y cálculos matemáticos
5)Matplotlib (matplotlib.pyplot) – Para visualizar las señales en el dominio del tiempo y frecuencia.
6)SciPy (scipy.signal) – Para procesamiento de señales, filtrado y análisis en frecuencia.

## RESULTADOS ESPERADOS

*Separación exitosa de las señales de voz, permitiendo escuchar por separado cada una de las voces grabadas.
*Análisis de la efectividad del método empleado y sugerencias de mejora.


