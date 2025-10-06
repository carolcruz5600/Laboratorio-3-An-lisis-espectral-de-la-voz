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

### **Señal de Voz Hombre 2**

Se utiliza un audio grabado por un hombre. Primero, se realizan los cálculos manuales para poder aplicar el filtro a la señal. Se utilizan frecuencias de $Ω_l = 80 Hz$ y $Ω_2 = 400 Hz$ que son frecuencias que se encuetran dentro del rango de la frecuencia fundamental esto se hace para eliminar ruido que esten fuera de esos valores recomendados a niveles de $-2dB$ para, también se utilizaron frecuencias de atenuación de $Ω_1 = 20 Hz$ y $Ω_2 = 550 Hz$ trabajadas a $-10dB$ para evitar recortes importantes o bruscos en la señales.
Se realizaron los gráficos para observar el comportamiento del filtro pasa banda, sin embargo se diseña primero un filtro pasa bajo ya que se tiene más facilidad al momento de trabajarlo permitiendo calcular la frecuencia de corte y el orden necesarios. Posterior, se realizar la función de transferencia y la transformación necesaria para convertirlo en un filtro pasa banda.

<p align="center">
<img width="561" height="898" alt="image" src="https://github.com/user-attachments/assets/cba4b637-6973-4efb-8cc1-42063270e5aa" />
</p>

### Implementación Filtro

>### Definición Variables y Señales Fijas
En esta parte se define valores necesarios para implementar el filtro:
```python
fs=48000
n=4
fl=80
fu=400
```
* $F_s$: Frecuencia de muestreo que se utiliza para la señal del audio $(48k Hz)$
* n: El orden que se calculo previamente para el filtro butterworth $(4)$
* $F_l$: Frecuencia baja del filtro de acuerdo a los valores recomendados $(80 Hz)$
* $F_l$: Frecuencia alta del filtro de acuerdo a los valores recomendados $(400 Hz)$

>### Leer la Señal y Aplicar Filtro
En el código se crea y aplica un filtro digital pasa banda digital a la señal de audio de voz del hombre y preparar une eje en el tiempo para su análisis, se normalizan las señales en ``wn`` que están entre 0 y 1. Luego con la función ``butter`` se diseñan los coegicientes b y acon los parametros necesarios para el filtro, con la función ``filtfilt`` aplicando a la señal que se guardo en ``x``, evitando un desfase, por lo que se genara la señal filtrada en y. Finalmente, se crea el vector ``t`` que representa el tiempo correspondiente a cada muestra de la señal filtrada, dividiendo los índices de las muestras por la frecuencia de muestreo fs, lo que permite graficar o analizar la señal en el dominio temporal.
```python
fs_read, x = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Hombre-2.wav")
wn = [fl/(fs/2), fu/(fs/2)]
b, a = butter(n, wn, btype='bandpass', analog=False)
# Aplicar el filtro
y = filtfilt(b, a, x)
# Crear eje de tiempo
t = np.arange(len(y)) / fs
```
>### Graficar la Señal Filtrada
En la parte decódigo se utiliza para graficar la señal de voz filtrada en función del tiempo, permitiendo visualizar cómo se comporta después del filtrado. Primero, se crea una figura de tamaño 10×4 pulgadas con plt.figure(figsize=(10,4)). Luego, plt.plot(t, y, color='orange') dibuja la señal y frente al tiempo t en color naranja. Se agregan título y etiquetas a los ejes con plt.title, plt.xlabel y plt.ylabel para indicar que el eje horizontal representa el tiempo en segundos y el vertical la amplitud de la señal. La función plt.grid(True) activa la cuadrícula para facilitar la lectura de valores, mientras que plt.tight_layout() ajusta automáticamente los márgenes de la figura. Finalmente, plt.show() muestra la gráfica en pantalla, permitiendo analizar visualmente la señal filtrada y verificar que las frecuencias no deseadas han sido atenuadas.
```python
plt.figure(figsize=(10,4))
plt.plot(t, y, color='orange')
plt.title('Señal de voz Hombre 2 filtrada en el tiempo')
plt.xlabel('Tiempo [s]')
plt.ylabel('Amplitud')
plt.grid(True)
plt.tight_layout()
plt.show()
```
>### Señal filtrada
La señal filtrada presenta una mejora notable en comparación con la señal original. En la gráfica se aprecia una forma de onda más limpia, con menor presencia de variaciones pequeñas e irregulares que suelen asociarse al ruido. Esto indica que el proceso de filtrado logró eliminar componentes no deseadas del sonido, manteniendo las partes esenciales de la voz. Además, la amplitud de la señal se mantiene dentro de un rango más estable y las secciones correspondientes a las sílabas o palabras son más claras y definidas. Esto sugiere que la energía del habla se conserva correctamente y que el filtro aplicado ha cumplido su función de atenuar frecuencias innecesarias o interferencias.
<img width="989" height="390" alt="image" src="https://github.com/user-attachments/assets/fd378801-f067-4e74-bb98-d090b447e179" />

### **Señal de Voz Mujer 3**

Al igual que la parte aanterior se utiliza un audio esta vez grabado por una mujer. Primero, se realizan los cálculos manuales para poder aplicar el filtro a la señal. Se utilizan frecuencias de $Ω_l = 150 Hz$ y $Ω_2 = 500 Hz$ que son frecuencias que se encuetran dentro del rango de la frecuencia fundamental esto se hace para eliminar ruido que esten fuera de esos valores recomendados a niveles de $-2dB$ para, también se utilizaron frecuencias de atenuación de $Ω_1 = 100 Hz$ y $Ω_2 = 7000 Hz$ trabajadas a $-10dB$ para evitar recortes importantes o bruscos en la señales.
Se realizaron los gráficos para observar el comportamiento del filtro pasa banda, sin embargo se diseña primero un filtro pasa bajo ya que se tiene más facilidad al momento de trabajarlo permitiendo calcular la frecuencia de corte y el orden necesarios. Posterior, se realizar la función de transferencia y la transformación necesaria para convertirlo en un filtro pasa banda.

<p align="center">
<img width="561" height="898" alt="image" src="https://github.com/user-attachments/assets/5b3840e1-84cd-4682-afc3-aaa762a08fd2" />
</p>

### Implementación Filtro

>### Definición Variables y Señales Fijas
En esta parte se define valores necesarios para implementar el filtro:
```python
fs=48000
n=3
fl=150
fu=500
```
* $F_s$: Frecuencia de muestreo que se utiliza para la señal del audio $(48k Hz)$
* n: El orden que se calculo previamente para el filtro butterworth $(3)$
* $F_l$: Frecuencia baja del filtro de acuerdo a los valores recomendados $(150 Hz)$
* $F_l$: Frecuencia alta del filtro de acuerdo a los valores recomendados $(500 Hz)$

>### Leer la Señal y Aplicar Filtro
En el código se crea y aplica un filtro digital pasa banda digital a la señal de audio de voz del hombre y preparar une eje en el tiempo para su análisis, se normalizan las señales en ``wn`` que están entre 0 y 1. Luego con la función ``butter`` se diseñan los coegicientes b y acon los parametros necesarios para el filtro, con la función ``filtfilt`` aplicando a la señal que se guardo en ``x``, evitando un desfase, por lo que se genara la señal filtrada en y. Finalmente, se crea el vector ``t`` que representa el tiempo correspondiente a cada muestra de la señal filtrada, dividiendo los índices de las muestras por la frecuencia de muestreo fs, lo que permite graficar o analizar la señal en el dominio temporal.
```python
fs_read, x = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Mujer-3.wav")
wn = [fl/(fs/2), fu/(fs/2)]
b, a = butter(n, wn, btype='bandpass', analog=False)
# Aplicar el filtro
y = filtfilt(b, a, x)
# Crear eje de tiempo
t = np.arange(len(y)) / fs
```
>### Graficar la Señal Filtrada
En la parte decódigo se utiliza para graficar la señal de voz filtrada en función del tiempo, permitiendo visualizar cómo se comporta después del filtrado. Primero, se crea una figura de tamaño 10×4 pulgadas con plt.figure(figsize=(10,4)). Luego, plt.plot(t, y, color='orange') dibuja la señal y frente al tiempo t en color naranja. Se agregan título y etiquetas a los ejes con plt.title, plt.xlabel y plt.ylabel para indicar que el eje horizontal representa el tiempo en segundos y el vertical la amplitud de la señal. La función plt.grid(True) activa la cuadrícula para facilitar la lectura de valores, mientras que plt.tight_layout() ajusta automáticamente los márgenes de la figura. Finalmente, plt.show() muestra la gráfica en pantalla, permitiendo analizar visualmente la señal filtrada y verificar que las frecuencias no deseadas han sido atenuadas.
```python
plt.figure(figsize=(10,4))
plt.plot(t, y, color='orange')
plt.title('Señal de voz Mujer 3 filtrada en el tiempo')
plt.xlabel('Tiempo [s]')
plt.ylabel('Amplitud')
plt.grid(True)
plt.tight_layout()
plt.show()
```
>### Señal filtrada
Tras el proceso de filtrado, la señal adquiere una forma más limpia y definida, con una reducción notable del ruido y una mejor representación de las características propias del habla. Las oscilaciones son más suaves y los picos excesivos se atenúan, conservando únicamente las variaciones relevantes de la voz. Esto demuestra que el filtrado fue adecuado, ya que logra mantener la información principal de la señal mientras elimina las distorsiones no deseadas.
<img width="989" height="390" alt="image" src="https://github.com/user-attachments/assets/a94e25dc-1a9c-4616-a77b-2c236aac11d0" />

### Jitter

El jitter se utiliza para medir qué tanto cambia la frecuencia de la voz de un ciclo al proximo. Cuando se habla, las cuerdas vocales generan muchas vibraciones por segundo, en condiciones ideales o normales cada vibración duraría lo mismo. Pero en la realidad siempre existen diferencias, estas se les conoce como jitter. Se usa para saber que tan estable o uniforme es la voz.
Existen dos tipos de jitter: **absoluto** que mide el cambio exacto entre un ciclo y el siguiente; **relativo** que lo compara con el promedio de todos los ciclos. Un valor alto indica que la voz es más inestable e irregula, mientras que para un valor bajo indica que la voz es más constante y controlada.
## Parte C
