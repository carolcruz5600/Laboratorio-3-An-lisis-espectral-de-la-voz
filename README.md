# Laboratorio 3: Análisis espectral de la voz

## Integrantes
* Laura Valentina Velásquez Castiblanco (5600846)
* Carol Valentina Cruz Becerra (5600845)
* Carlos Felipe Moreno Guzmán (5600881)

## Objetivos:
* Capturar y procesar señales de voz masculinas y femeninas.
* Aplicar la Transformada de Fourier como herramienta de análisis espectral de la voz.
* Extraer parámetros característicos de la señal de voz: frecuencia fundamental, frecuencia media, brillo, intensidad, jitter y shimmer.
* Comparar las diferencias principales entre señales de voz de hombres y mujeres a partir de su análisis en frecuencia.
* Desarrollar conclusiones sobre el comportamiento espectral de la voz humana en función del género. 

## Configuración inicial

Para la parte A y B se necesitan el uso de librerias:
```python
from scipy.io import wavfile
from scipy.fft import fft, fftfreq
import matplotlib.pyplot as plt
import numpy as np
from scipy.signal import butter, filtfilt
from scipy.signal import find_peaks
```
* scipy.io: Para poder leer y escribir los archivos de audio ``.wav``.
* scipy.fft: Se usa para poder trabajar con la transformada de fourier.
* matplotlib.pyplot: Para poder realizar las diferentes graficas de las señales, transformadas y las señales filtradas.
* numpy: Para realizar los cálculos numéricos necesarios.
* scipy.signal: Se utiliza para realizar los filtros a las señales escogidas. 
## Parte A

## Parte B

### Señal de Voz Hombre 2

Se utiliza un audio grabado por un hombre. Primero, se realizan los cálculos manuales para poder aplicar el filtro a la señal. Se utilizan frecuencias de $Ω_l = 80 Hz$ y $Ω_2 = 400 Hz$ que son frecuencias que se encuetran dentro del rango de la frecuencia fundamental esto se hace para eliminar ruido que esten fuera de esos valores recomendados a niveles de $-2dB$ para, sin embargo también se utilizaron frecuencias de atenuación de $Ω_1 = 20 Hz$ y $Ω_2 = 550 Hz$ trabajadas a $-10dB$, se realizan los gráficos para observar como se comporta el filtro pasa banda, sin embargo se diseña primero un filtro pasa bajo ya que se conoce como realizarlo para calcular la frecuencia de corte necesaria a esas frecuencias, el orden y luego realizar la función de transferencia con la transformación necesaria para ser un filtro pasa banda.

<p align="center">
<img width="561" height="898" alt="image" src="https://github.com/user-attachments/assets/cba4b637-6973-4efb-8cc1-42063270e5aa" />
</p>

## Parte C
