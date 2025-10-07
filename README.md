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
## Diagramas de Flujo
>### Parte A
<p align="center">
<img width="538" height="1280" alt="image" src="https://github.com/user-attachments/assets/2e19968b-f23d-4cc9-a8e1-18aac5e44c73" />
</p>

>### Parte B

<p align="center">
<img width="598" height="1280" alt="image" src="https://github.com/user-attachments/assets/15ef07ed-c13d-4ec4-be82-e5ea49452b31" />
</p>

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
  
# **Parte A**
## **1 y 2. Grabación y Almacenamiento en .wav**

Con el propósito de realizar el pertinente análisis frecuencial, se grabó con un el micrófono de un celular la voz de 6 estudiantes de la Universidad Militar Nueva Granada, tres hombres y tres mujeres de aproximadamente 20 años de edad. La frase que se indicó a los estudiantes que repitieran fue: ***"La química se basa en cuestiones simples"***. Se utilizó el microfono de un teléfono *Android* para adquirir los archivos de audio, que posteriormente fueron convertidos al formato `.wav` requerido para la implementación en *Google Colab*. Estos fueron guardados con un identificador que permitió su identificación tanto de género como de número (`géneronúmero.wav`) para facilitar la identificación en el código.

Para la implementación en *Google Colab*, en primera instancia de cargaron los archivos `.wav` en la unidad de *Drive*, para posteriormente ser leídos. Se muestra a continuación la sintaxis para cargar el contenido del *Drive* al código:

```python
from google.colab import drive
drive.mount('/content/drive')
```
La lectura de cada una de las señales se realizó con la ayuda del módulo `scipy.io.wavfile` de la siguiente manera:

```python
fs1, hombre1 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Hombre-1.wav")
```
Donde `fs1` almacena la frecuencia de muestreo de la señal y `hombre1` crea un array que guarda la señal de audio. De esta manera, se identificó la frecuencia de muestreo estándar del micrófono, $f_s=48000 Hz$ para todas las señales. Se realizó este procedimiento de manera secuencial para las señales restantes.

## **3. Gráficas en el Dominio del Tiempo**
Con las señales ya almacenadas en arrays, se procedió a graficar cada una de ellas utilizando `NumPy` y `matplotlib.pyplot` como se muestra a continuación:

```python
n1 = np.arange(len(hombre1))
plt.figure(figsize=(10, 4))
plt.stem(n1, hombre1)
plt.xlabel("n (muestras)")
plt.ylabel("Amplitud")
plt.title("Hombre 1")
plt.grid(True)
plt.show()
```
> [!NOTE]
> Nótese la creación del array `n1` del eje de muestras $n$ para poder graficar la señal, usando la función `np.arange()` del tamaño de la señal dado por `len()`.

### Hombre 1
<img width="891" height="393" alt="image" src="https://github.com/user-attachments/assets/73961c70-34f1-4c8f-8d96-b10690588bad" />

### Hombre 2
<img width="879" height="393" alt="image" src="https://github.com/user-attachments/assets/5e09de5c-e577-4405-b013-2d750f76bb66" />

### Hombre 3
<img width="879" height="393" alt="image" src="https://github.com/user-attachments/assets/b76c0d63-9878-495b-95f7-a627d6dedab3" />

### Mujer 1
<img width="879" height="393" alt="image" src="https://github.com/user-attachments/assets/c5d99294-d37e-48a0-832d-16ea04af9344" />

### Mujer 2
<img width="879" height="393" alt="image" src="https://github.com/user-attachments/assets/13709577-0795-41cb-8721-ab8225e7120f" />

### Mujer 3
<img width="879" height="393" alt="image" src="https://github.com/user-attachments/assets/2f05eeec-018b-4a1a-9925-5f788e9f0c00" />

## **4. Transformadas de Fourier (FFT´s)**
Una vez importados todos los archivos `.wav` y graficados en el dominio del tiempo, se procedió a analizar las señales de audio en el dominio de la frecuencia mediante la Transformada Rápida de Fourier (FFT).  Para ello, se usó el módulo `numpy.fft`, que contiene todas las funciones relacionadas con la FFT.
```python
fft_hombre1 = fft(hombre1)
nh1 = len(hombre1)
freq_hombre1 = fftfreq(nh1, 1/fs1)
fft_hombre1 = np.abs(fft_hombre1)
fft_hombre1 = fft_hombre1[:nh1//2]
freq_hombre1 = freq_hombre1[:nh1//2]
```
* `fft_hombre1`: Magnitud
* `freq_hombre1`: Frecuencias

> [!NOTE]
> La magnitud de la FFT se calcula con `np.abs(fft_hombre1)`,  la mitad positiva tanto del eje de frecuencias y el de las magnitudes se obtienen tomando la mitad inferior de los arrays dada por `nh1//2`.

Con ambos ejes de la FFT, su gráfica se realizó empleando la siguiente sintaxis de manera secuencial para cada señal:
```python
plt.plot(freq_hombre1, fft_hombre1)
plt.xlabel("Frecuencia (Hz)")
plt.ylabel("Amplitud")
plt.title("Hombre 1")
plt.xlim(0, 1000) #muestra de 0 a 1000 Hz
plt.grid(True)
plt.show()
```
> [!IMPORTANT]
> Para poder ver con mayor detalle las componentes espectrales más relevantes, se tomó el eje de frecuencias de $0$ a $1000$ $Hz$. Este intervalo concentra la mayor parte de la energía de las señales de voz humanas, así que un zoom a esta zona permite identificar los picos frecuenciales que pueden representar la frecuencia fundamental y sus armónicos.

### Hombre 1
<img width="584" height="455" alt="image" src="https://github.com/user-attachments/assets/46085dfb-c995-41d3-9f65-8295120f2fe8" />

### Hombre 2
<img width="584" height="455" alt="image" src="https://github.com/user-attachments/assets/511327d3-b5d0-4fca-b024-e58e4d7a9719" />

### Hombre 3
<img width="593" height="455" alt="image" src="https://github.com/user-attachments/assets/ee828633-0d33-4455-874b-5a15382eda5e" />

### Mujer 1
<img width="584" height="455" alt="image" src="https://github.com/user-attachments/assets/ccb0d9a3-e4c8-4da5-aee0-fb1c0f4a8d42" />

### Mujer 2
<img width="593" height="455" alt="image" src="https://github.com/user-attachments/assets/40a72846-60b4-452d-8687-735b8cabae6b" />

### Mujer 3
<img width="593" height="455" alt="image" src="https://github.com/user-attachments/assets/099bca0a-705b-44cb-ae7f-d9259d57c276" />

## **5. Características de la Señal**
Para concluir esta sección, luego de obtener las FFT de las señales, se procedió a extraer algunos parámetros característicos que permiten describir el contenido espectral y energético de las FFT. Estos indicadores son esenciales en el análisis de la voz, ya que permiten identificar diferencias entre la voz de los hablantes y comprender la variación de las propiedades acústicas en función del género o las características fisiológicas.

> Frecuencia Fundamental

Corresponde al primer pico principal del espectro de magnitudes de una señal. Representa la frecuencia de vibración dominante, que a su vez se asocia con el tono de voz percibido. Para calcularla en `python`, se determinó una función que identifica la posición donde se encuentra el pico mas alto en el espectro de la FFT, luego se asocia con la frecuencia correspondiente:
```python
# Función
def f_fund(fft_magnitud, freqs):
    idx = np.argmax(fft_magnitud[1:]) + 1
    freq_fund = freqs[idx]
    return freq_fund
# Cálculo
ff_h1 = f_fund(fft_hombre1, freq_hombre1)
```
#### Resultados

* **Hombre 1:** $577.184$ $Hz$
* **Hombre 2:** $571.948$ $Hz$
* **Hombre 3:** $499.362$ $Hz$
* **Mujer 1:** $246.41$ $Hz$
* **Mujer 2:** $446.493$ $Hz$
* **Mujer 3:** $422.917$ $Hz$
  
> Frecuencia Media

Representa el promedio ponderado de las frecuencias que componen una señal. Indica la región del espectro donde se concentra la mayor parte de la energía de la señal. Matemáticamente se puede obtener mediante la siguiente expresión:

$$f_{media}=\frac{\sum(|X(f)|\cdot f)}{\sum|X(f)|}$$

Donde $|X(f)|$ es la magnitud de la FFT y $f$ es la frecuencia. En `python`, se implementó una función que realiza el cálculo y que arroja $0$ si la señal el silenciosa, para evitar la división entre $0$.

```python
# Función
def f_media(fft_magnitud, freqs):
    if np.sum(fft_magnitud) == 0:
        return 0
    return np.sum(freqs * fft_magnitud) / np.sum(fft_magnitud)
# Cálculo
fm_h1 = f_media(fft_hombre1, freq_hombre1)
```
#### Resultados

* **Hombre 1:** $3142.4$ $Hz$
* **Hombre 2:** $3080.579$ $Hz$
* **Hombre 3:** $4487.849$ $Hz$
* **Mujer 1:** $3972.601$ $Hz$
* **Mujer 2:** $4866.953$ $Hz$
* **Mujer 3:** $5008.831$ $Hz$

> Brillo

Describe la proporción de energía concentrada en las frecuencias altas de una señal. En el ámbito de la voz humana, un mayor brillo indica un tono más agudo, mientras que un brillo bajo indica un sonido más grave. Está definido mediante la siguiente expresión:

$$B=\frac{\sum_{f>f_c}|X(f)|}{\sum|X(f)|}$$

Donde $|X(f)|$ es la magnitud de la FFT y $f_c$ es la **frecuencia de corte** que delimita el rango considerado como "alta frecuencia". En este caso, se usó $f_c=1500$ $Hz$. En `python` se definió una función que calcula la relación entre la energía alta (a partir de $1500$ $Hz$) y la energía total del espectro. Nuevamente arroja $0$ si la señal es silenciosa.
```python
# Función
def brillo(fft_magnitud, freqs, f_corte=1500):
    energia_total = np.sum(fft_magnitud)
    energia_alta = np.sum(fft_magnitud[freqs > f_corte])
    if energia_total == 0:
        return 0
    return energia_alta / energia_total
# Cálculo
b_h1 = brillo(fft_hombre1, freq_hombre1)
```
#### Resultados

* **Hombre 1:** $0.488$
* **Hombre 2:** $0.439$
* **Hombre 3:** $0.474$
* **Mujer 1:** $0.469$
* **Mujer 2:** $0.63$
* **Mujer 3:** $0.55$

> Intensidad (energía)

Indica cuanto contribuye cada componente espectral de frecuencia a una señal. Una mayor energía en determinada frecuencia representa que gran parte de la energía total de la señal corresponde a esa frecuencia en específico. Está dada por:

$$E=\sum_{k=0}^{N-1}|X(k)|^2$$

Donde $|X(k)|$ es la magnitud de la componente frecuencial en $k$ y $N$ es el número total de muestras. En `python` simplemente se creó una función que aplica la ecuación teniendo como parámetro la magnitud de la FFT.
```python
# Función
def energia(fft_magnitud):
    return np.sum(fft_magnitud ** 2)
# Cálculo
e_h1 = energia(fft_hombre1)
```
#### Resultados

* **Hombre 1:** $8.265 \times 10^{16}$
* **Hombre 2:** $1.068 \times 10^{17}$
* **Hombre 3:** $8.668 \times 10^{16}$
* **Mujer 1:** $4.494 \times 10^{16}$
* **Mujer 2:** $6.399 \times 10^{16}$
* **Mujer 3:** $6.436 \times 10^{16}$

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

>### Explicación General del Código
Primero, se lee el archivo de audio y se obtiene la señal junto con su frecuencia de muestreo. Luego, se detectan los picos de la señal que corresponden a los ciclos de vibración de las cuerdas vocales, asegurando que la distancia mínima entre picos sea coherente con la frecuencia máxima esperada (400 Hz-500 Hz). A partir de los picos se calculan los intervalos de tiempo entre ciclos consecutivos y, usando estos valores, se determina el jitter absoluto como la media de las diferencias entre intervalos y el jitter relativo como un porcentaje respecto al promedio de los intervalos. Finalmente, se imprimen ambos valores, permitiendo evaluar la regularidad y estabilidad de la voz.

>### Hombre 1

```python
fs1, hombre1 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Hombre-1.wav")
distancia_min = int(fs1 / 400)
picos, _ = find_peaks(hombre1, distance=distancia_min)
tiempos = picos / fs1
Ti = np.diff(tiempos)
if len(Ti) < 2:
  print("Error: No se detectaron suficientes ciclos para calcular jitter.")
else:
  diferencias = np.abs(np.diff(Ti))
  jitter_absoluto = np.mean(diferencias)  # en segundos
  jitter_relativo = (jitter_absoluto / np.mean(Ti)) * 100
  print(f"Jitter Absoluto (Hombre 1): {jitter_absoluto:.6f} s")
  print(f"Jitter Relativo (Hombre 1): {jitter_relativo:.3f} %")
```
#### Resultados

``Jitter Absoluto (Hombre 1): 0.000728 s``

``Jitter Relativo (Hombre 1): 21.138 %``

>### Hombre 2

```python
fs2, hombre2 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Hombre-2.wav")
distancia_min = int(fs2 / 400)
picos, _ = find_peaks(hombre2, distance=distancia_min)
tiempos = picos / fs2
Ti = np.diff(tiempos)
if len(Ti) < 2:
  print("Error: No se detectaron suficientes ciclos para calcular jitter.")
else:
  diferencias = np.abs(np.diff(Ti))
  jitter_absoluto = np.mean(diferencias)  # en segundos
  jitter_relativo = (jitter_absoluto / np.mean(Ti)) * 100
  print(f"Jitter Absoluto (Hombre 2): {jitter_absoluto:.6f} s")
  print(f"Jitter Relativo (Hombre 2): {jitter_relativo:.3f} %")
```
#### Resultados

``Jitter Absoluto (Hombre 2): 0.000785 s``

``Jitter Relativo (Hombre 2): 23.121 %``

>### Hombre 3

```python
fs3, hombre3 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Hombre-3.wav")
distancia_min = int(fs3 / 400)
picos, _ = find_peaks(hombre3, distance=distancia_min)
tiempos = picos / fs3
Ti = np.diff(tiempos)
if len(Ti) < 2:
  print("Error: No se detectaron suficientes ciclos para calcular jitter.")
else:
  diferencias = np.abs(np.diff(Ti))
  jitter_absoluto = np.mean(diferencias)  # en segundos
  jitter_relativo = (jitter_absoluto / np.mean(Ti)) * 100
  print(f"Jitter Absoluto (Hombre 3): {jitter_absoluto:.6f} s")
  print(f"Jitter Relativo (Hombre 3): {jitter_relativo:.3f} %")
```
#### Resultados

``Jitter Absoluto (Hombre 3): 0.000842 s``

``Jitter Relativo (Hombre 3): 24.141 %``

>### Mujer 1

```python
fsm1, mujer1 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Mujer-1.wav")
distancia_min = int(fsm1 / 500)
picos, _ = find_peaks(mujer1, distance=distancia_min)
tiempos = picos / fsm1
Ti = np.diff(tiempos)
if len(Ti) < 2:
  print("Error: No se detectaron suficientes ciclos para calcular jitter.")
else:
  diferencias = np.abs(np.diff(Ti))
  jitter_absoluto = np.mean(diferencias)  # en segundos
  jitter_relativo = (jitter_absoluto / np.mean(Ti)) * 100
  print(f"Jitter Absoluto (Mujer 1): {jitter_absoluto:.6f} s")
  print(f"Jitter Relativo (Mujer 1): {jitter_relativo:.3f} %")
```
#### Resultados

``Jitter Absoluto (Mujer 1): 0.000584 s``

``Jitter Relativo (Mujer 1): 21.297 %``

>### Mujer 2

```python
fsm2, mujer2 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Mujer-2.wav")
distancia_min = int(fsm2 / 400)
picos, _ = find_peaks(mujer2, distance=distancia_min)
tiempos = picos / fsm2
Ti = np.diff(tiempos)
if len(Ti) < 2:
  print("Error: No se detectaron suficientes ciclos para calcular jitter.")
else:
  diferencias = np.abs(np.diff(Ti))
  jitter_absoluto = np.mean(diferencias)  # en segundos
  jitter_relativo = (jitter_absoluto / np.mean(Ti)) * 100
  print(f"Jitter Absoluto (Mujer 2): {jitter_absoluto:.6f} s")
  print(f"Jitter Relativo (Mujer 2): {jitter_relativo:.3f} %")
```
#### Resultados

``Jitter Absoluto (Mujer 2): 0.000644 s``

``Jitter Relativo (Mujer 2): 17.806 %``

>### Mujer 3

```python
fsm3, mujer3 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Mujer-3.wav")
distancia_min = int(fsm3 / 500)
picos, _ = find_peaks(mujer3, distance=distancia_min)
tiempos = picos / fsm3
Ti = np.diff(tiempos)
if len(Ti) < 2:
  print("Error: No se detectaron suficientes ciclos para calcular jitter.")
else:
  diferencias = np.abs(np.diff(Ti))
  jitter_absoluto = np.mean(diferencias)  # en segundos
  jitter_relativo = (jitter_absoluto / np.mean(Ti)) * 100
  print(f"Jitter Absoluto (Mujer 2): {jitter_absoluto:.6f} s")
  print(f"Jitter Relativo (Mujer 2): {jitter_relativo:.3f} %")
```
#### Resultados

``Jitter Absoluto (Mujer 3): 0.000568 s``

``Jitter Relativo (Mujer 3): 21.214 %``

### Shimmer

El shimmer indica como varia la amplitud de la voz de un ciclo a otro. Como se menciona anteriormente las cuerdas vocales vibran mucho por segundo, cada vibración produce un pico de amplitud que si la voz es completamente uniforme cada pico tendría la misma altura, pero en condiciones normales siempre existen diferencias también. Las diferencias en amplitudes de los ciclos se conocen cómo shimmers. Dependiendo de un shimmer bajo significa que la voz es más estable, mientras que uno alto indican fluctuaciones estando relacionados con fatigas vocales. 

>### Explicación General del Código
Primero, se lee el archivo de audio y se normaliza la señal para que sus valores estén entre -1 y 1, facilitando los cálculos posteriores. Luego, se detectan los picos de la señal, que representan los momentos de mayor amplitud de cada ciclo de vibración de las cuerdas vocales. Se establece una distancia entre picos para evitar detectar falsos máximos y asegurarse de que solo se consideren los ciclos reales de la voz. A partir de estos picos se extraen las amplitudes correspondientes. Finalmente, el código calcula el shimmer absoluto, que es la variación promedio de amplitud entre ciclos consecutivos, y el shimmer relativo, que expresa esa variación como un porcentaje respecto al promedio de todas las amplitudes.

>### Hombre 1

```python
fshs1, h1 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Hombre-1.wav")
h1 = h1 / np.max(np.abs(h1))
distancia_min = int(fshs1 / 80)
picos, _ = find_peaks(h1, distance=distancia_min)
amplitudes = h1[picos]
if len(amplitudes) < 2:
    print("Error: No se detectaron suficientes ciclos para calcular shimmer.")
else:
    diferencias = np.abs(np.diff(amplitudes))
    shimmer_abs = np.mean(diferencias)
    shimmer_rel = (shimmer_abs / np.mean(amplitudes)) * 100

    print("Shimmer Hombre 1")
    print(f"Shimmer Absoluto (amplitud): {shimmer_abs:.3f}")
    print(f"Shimmer Relativo (%): {shimmer_rel:.3f}")
    print(f"Amplitud Media: {np.mean(amplitudes):.3f}")
    print(f"Número de Ciclos: {len(amplitudes)}")
```
#### Resultados
``Shimmer Absoluto (amplitud): 0.060``

``Shimmer Relativo (%): 37.284``

``Amplitud Media: 0.161``

``Número de Ciclos: 213``

>### Hombre 2

```python
fshs2, h2 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Hombre-2.wav")
h2 = h2 / np.max(np.abs(h2))
distancia_min = int(fshs2 / 80)
picos, _ = find_peaks(h2, distance=distancia_min)
amplitudes = h2[picos]
if len(amplitudes) < 2:
    print("Error: No se detectaron suficientes ciclos para calcular shimmer.")
else:
    diferencias = np.abs(np.diff(amplitudes))
    shimmer_abs = np.mean(diferencias)
    shimmer_rel = (shimmer_abs / np.mean(amplitudes)) * 100

    print("Shimmer Hombre 2")
    print(f"Shimmer Absoluto (amplitud): {shimmer_abs:.3f}")
    print(f"Shimmer Relativo (%): {shimmer_rel:.3f}")
    print(f"Amplitud Media: {np.mean(amplitudes):.3f}")
    print(f"Número de Ciclos: {len(amplitudes)}")
```
#### Resultados

``Shimmer Absoluto (amplitud): 0.086``

``Shimmer Relativo (%): 41.858``

``Amplitud Media: 0.205``

``Número de Ciclos: 168``

>### Hombre 3

```python
fshs3, h3 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Hombre-3.wav")
h3 = h3 / np.max(np.abs(h3))
distancia_min = int(fshs3 / 80)
picos, _ = find_peaks(h3, distance=distancia_min)
amplitudes = h3[picos]
if len(amplitudes) < 2:
    print("Error: No se detectaron suficientes ciclos para calcular shimmer.")
else:
    diferencias = np.abs(np.diff(amplitudes))
    shimmer_abs = np.mean(diferencias)
    shimmer_rel = (shimmer_abs / np.mean(amplitudes)) * 100

    print("Shimmer Hombre 3")
    print(f"Shimmer Absoluto (amplitud): {shimmer_abs:.3f}")
    print(f"Shimmer Relativo (%): {shimmer_rel:.3f}")
    print(f"Amplitud Media: {np.mean(amplitudes):.3f}")
    print(f"Número de Ciclos: {len(amplitudes)}")
```
#### Resultados

``Shimmer Absoluto (amplitud): 0.082``

``Shimmer Relativo (%): 47.956``

``Amplitud Media: 0.171``

``Número de Ciclos: 185``

>### Mujer 1

```python
fsms1, m1 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Mujer-1.wav")
m1 = m1 / np.max(np.abs(m1))
distancia_min = int(fsms1 / 80)
picos, _ = find_peaks(m1, distance=distancia_min)
amplitudes = m1[picos]
if len(amplitudes) < 2:
    print("Error: No se detectaron suficientes ciclos para calcular shimmer.")
else:
    diferencias = np.abs(np.diff(amplitudes))
    shimmer_abs = np.mean(diferencias)
    shimmer_rel = (shimmer_abs / np.mean(amplitudes)) * 100

    print("Shimmer Mujer 1")
    print(f"Shimmer Absoluto (amplitud): {shimmer_abs:.3f}")
    print(f"Shimmer Relativo (%): {shimmer_rel:.3f}")
    print(f"Amplitud Media: {np.mean(amplitudes):.3f}")
    print(f"Número de Ciclos: {len(amplitudes)}")
```
#### Resultados

``Shimmer Absoluto (amplitud): 0.053``

``Shimmer Relativo (%): 33.451``

``Amplitud Media: 0.159``

``Número de Ciclos: 194``

>### Mujer 2

```python
fsms2, m2 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Mujer-2.wav")
m2 = m2 / np.max(np.abs(m2))
distancia_min = int(fsms2 / 150)
picos, _ = find_peaks(m2, distance=distancia_min)
amplitudes = m2[picos]
if len(amplitudes) < 2:
    print("Error: No se detectaron suficientes ciclos para calcular shimmer.")
else:
    diferencias = np.abs(np.diff(amplitudes))
    shimmer_abs = np.mean(diferencias)
    shimmer_rel = (shimmer_abs / np.mean(amplitudes)) * 100

    print("Shimmer Mujer 2")
    print(f"Shimmer Absoluto (amplitud): {shimmer_abs:.3f}")
    print(f"Shimmer Relativo (%): {shimmer_rel:.3f}")
    print(f"Amplitud Media: {np.mean(amplitudes):.3f}")
    print(f"Número de Ciclos: {len(amplitudes)}")
```
#### Resultados

``Shimmer Absoluto (amplitud): 0.060``

``Shimmer Relativo (%): 39.900``

``Amplitud Media: 0.150``

``Número de Ciclos: 235``

>### Mujer 3

```python
fsms3, m3 = wavfile.read("/content/drive/MyDrive/Colab Notebooks/Lab Procesamiento Digital de Señales/PDS - Lab 3/Mujer-3.wav")
m3 = m3 / np.max(np.abs(m3))
distancia_min = int(fsms3 / 80)
picos, _ = find_peaks(m3, distance=distancia_min)
amplitudes = m3[picos]
if len(amplitudes) < 2:
    print("Error: No se detectaron suficientes ciclos para calcular shimmer.")
else:
    diferencias = np.abs(np.diff(amplitudes))
    shimmer_abs = np.mean(diferencias)
    shimmer_rel = (shimmer_abs / np.mean(amplitudes)) * 100

    print("Shimmer Mujer 3")
    print(f"Shimmer Absoluto (amplitud): {shimmer_abs:.3f}")
    print(f"Shimmer Relativo (%): {shimmer_rel:.3f}")
    print(f"Amplitud Media: {np.mean(amplitudes):.3f}")
    print(f"Número de Ciclos: {len(amplitudes)}")
```

``Shimmer Absoluto (amplitud): 0.075``

``Shimmer Relativo (%): 49.579``

``Amplitud Media: 0.151``

``Número de Ciclos: 182``

## Parte C

### **1. Registro de Adquisición**
| **Parámetro**              | **Valor/Descripción**           | **Observaciones**                          |
|-------------------------|-----------------------------|----------------------------------------|
| Frecuencia de muestreo (Fs) | 48000 Hz                  | Uniforme en todas las muestras         |
| Resolución              | 16 bits (estándar WAV)      | Rango dinámico adecuado                 |
| Canales                 | Mono (1 canal)              | Array unidimensional                   |
| Duración promedio       | ~3 segundos                 | Variable entre 2.75 – 3.54 s           |
| Condiciones ambientales | Controladas                 | Sin ruido ambiental significativo      |
| Calidad general         | Buena                       | Sin saturación detectada               |


Las grabaciones se realizaron en condiciones controladas con buena calidad de señal. No obstante, se evidenció variabilidad en la duración y en la amplitud de las muestras, lo cual puede asociarse a diferencias en la ejecución de cada registro. Para mantener cierta homogeneidad, la distancia al micrófono se midió de manera aproximada utilizando una regla de 10 cm desde el pecho.


### **2. Análisis Comparativo: Voces Masculinas vs Femeninas**

>### 2.1 Tabla de Resultados por Grupo

| Sujeto            | F0 (Hz) | F Media (Hz) | Brillo | Energía (×10¹⁵) | Jitter (%) | Shimmer (%) |
|-------------------|---------|--------------|--------|-----------------|------------|-------------|
| Hombre 1          | 577.18  | 3142.40      | 0.488  | 82.65           | 21.14      | 37.28       |
| Hombre 2          | 571.95  | 3080.58      | 0.439  | 106.80          | 23.12      | 41.86       |
| Hombre 3          | 499.36  | 4487.85      | 0.474  | 86.68           | 24.14      | 47.96       |
| Mujer 1           | 246.41  | 3972.60      | 0.469  | 44.94           | 21.30      | 33.45       |
| Mujer 2           | 446.49  | 4866.95      | 0.630  | 63.99           | 17.81      | 39.90       |
| Mujer 3           | 422.92  | 5008.83      | 0.550  | 64.36           | 21.21      | 49.58       |
| **Promedio Hombres** | **549.50** | **3570.28** | **0.467** | **92.04** | **22.80** | **42.37** |
| **Promedio Mujeres** | **371.94** | **4616.13** | **0.550** | **57.76** | **20.11** | **40.98** |

>### 2.2 Gráficos Comparativos

>### Gráfico 1:

<img width="806" height="343" alt="image" src="https://github.com/user-attachments/assets/23863567-4e4e-4f88-9155-8929cb52703f" />
<br>
<br>

El gráfico de frecuencia fundamental muestra valores considerablemente más altos en el grupo masculino (promedio ``549.5 Hz``, rango ``499-577 Hz``) comparado con el grupo femenino (promedio ``371.9 Hz``, rango ``246-446 Hz``). Sin embargo, estos valores son fisiológicamente anómalos, debido a que en condiciones normales las voces masculinas deberían presentar F0 entre ``85-180 Hz`` y las femeninas entre ``165-255 Hz``. La inversión observada (hombres con valores más altos que mujeres) confirma que el algoritmo está detectando armónicos de alta energía en lugar de la frecuencia fundamental real, particularmente pronunciado en las muestras masculinas donde los armónicos superiores tienen mayor amplitud espectral.

>### Gráfico 2:

<img width="799" height="349" alt="image" src="https://github.com/user-attachments/assets/a7d5278d-89d2-478a-9cd2-782aa3f71c17" />
<br>
<br>

La frecuencia media muestra una clara diferenciación entre grupos: las mujeres presentan valores consistentemente más altos (promedio ``4,616 Hz``, rango ``3,973-5,009 Hz``) que los hombres (promedio ``3,570 Hz``, rango ``3,081-4,488 Hz``). Este resultado es coherente con las diferencias anatómicas del tracto vocal, donde la menor longitud en mujeres desplaza el centro de gravedad espectral hacia frecuencias superiores. La dispersión dentro de cada grupo es moderada, con el Hombre 3 mostrando valores atípicamente elevados que se acercan al rango femenino, posiblemente debido a características individuales de su configuración vocal.

>### Gráfico 3:

<img width="803" height="342" alt="image" src="https://github.com/user-attachments/assets/ef0979ed-5a0a-48de-8fdc-d1c978dc78ee" />
<br>
<br>

El brillo espectral evidencia mayor concentración de energía en altas frecuencias para el grupo femenino (promedio ``0.550``, rango ``0.469-0.630``) comparado con el masculino (promedio ``0.467``, rango ``0.439-0.488``). Los datos muestran separación clara entre grupos con solapamiento mínimo, indicando que este parámetro es un buen discriminador entre voces masculinas y femeninas. La Mujer 2 presenta el valor más alto de brillo (0.630), sugiriendo una voz particularmente aguda o con mayor contenido armónico en el rango superior a ``1500 Hz``, mientras que el Hombre 2 muestra el menor brillo (``0.439``), caracterizando una voz más "opaca" o con predominancia de frecuencias bajas.

>### Gráfico 4:

<img width="802" height="355" alt="image" src="https://github.com/user-attachments/assets/60ac8227-0344-41e1-8179-e567434de9af" />
<br>
<br>

La energía total revela mayor intensidad en las grabaciones masculinas (promedio ``92.04×10¹⁵``, rango ``82.65-106.8×10¹⁵``) en comparación con las femeninas (promedio ``57.76×10¹⁵``, rango ``44.94-64.36×10¹⁵``), con una diferencia aproximada del 38%. La considerable variabilidad observada, especialmente en el grupo masculino donde el Hombre 2 alcanza valores significativamente superiores, sugiere que este parámetro está influenciado tanto por factores fisiológicos (capacidad de presión subglótica, masa de cuerdas vocales) como por variables metodológicas no controladas (distancia al micrófono, ganancia de grabación, volumen de fonación).

>### Gráfico 5:

<img width="811" height="361" alt="image" src="https://github.com/user-attachments/assets/e7679297-679a-4b4d-be29-bb0075bb88ac" />
<br>
<br>

El jitter relativo muestra valores similares entre ambos grupos, con promedios de ``22.80%`` en hombres (rango ``21.14-24.14%``) y ``20.11%`` en mujeres (rango ``17.81-21.30%``). La distribución homogénea y la ausencia de diferencias significativas entre géneros contrasta con las expectativas fisiológicas, pero más importante aún, los valores absolutos exceden dramáticamente los rangos clínicos normales (``<1%``) e incluso patológicos severos (``>5%``). Esta consistencia en valores anormalmente elevados en todos los participantes confirma un error sistemático en el algoritmo de detección de ciclos glóticos, independiente del género del hablante, indicando que la metodología requiere corrección antes de que estos resultados puedan considerarse válidos clínicamente.

>### Gráfico 5:

<img width="826" height="354" alt="image" src="https://github.com/user-attachments/assets/e0ea2d75-d32c-4e77-9e93-fccd163eb9f3" />
<br>
<br>

El shimmer relativo muestra valores elevados en ambos grupos con promedios muy similares: ``42.37%`` en hombres (rango ``37.28-47.96%``) y ``40.98%`` en mujeres (rango ``33.45-49.58%``), con una diferencia de apenas ``3.3%``. La dispersión dentro de cada grupo es considerable, especialmente en las mujeres donde los valores oscilan entre ``33.45%`` y ``49.58%``. Sin embargo, estos valores exceden los rangos clínicos normales (``<3-5%``).

> [!IMPORTANT]  
> Los valores de F0, Jitter y Shimmer presentan anomalías significativas. F0 esperado: ``5-180 Hz`` (hombres), ``165-255 Hz`` (mujeres). Jitter normal: ``<1.5%``. Shimmer normal ``<3-5%``

>### 2.3 Diferencias Observadas en Frecuencia Fundamental

| Grupo   | Rango de Referencia Literatura | Valor Obtenido | Observación                    |
|---------|--------------------------------|----------------|--------------------------------|
| Hombres | 85–180 Hz                      | 549.5 Hz       | Valor superior al rango típico |
| Mujeres | 165–255 Hz                     | 371.9 Hz       | Valor superior al rango típico |

<br>

>[!NOTE]
>Los valores obtenidos se encuentran por encima de los rangos reportados en la literatura para voces adultas. El método de detección basado en máximo de FFT puede ser sensible a componentes armónicos de mayor amplitud en el espectro, lo que puede influir en los valores registrados.

>### 2.4 Diferencias en términos de Brillo, Frecuencia Media e Intensidad

**Frecuencia Media**: Las mujeres presentaron valores 29.3% mayores (``4,616 Hz`` vs ``3,570 Hz``), reflejando el desplazamiento espectral hacia frecuencias altas debido a un tracto vocal más corto (``~15 cm`` vs ``~17 cm`` en hombres). Este resultado es consistente con la fisiología vocal.

**Brillo Espectral**: Las mujeres mostraron ``17.8%`` más brillo (``0.550`` vs ``0.467``), indicando mayor concentración de energía por encima de ``1500 Hz``. Esto explica perceptualmente por qué las voces femeninas suenan más "agudas" o "brillantes". Este resultado también es consistente y esperado.

**Intensidad**: Los hombres registraron ``38%`` mayor energía. Aunque puede reflejar diferencias fisiológicas (mayor masa de cuerdas vocales), sin normalización de volumen no es posible distinguir entre diferencias reales y artefactos de grabación (distancia al micrófono, ganancia variable).

### **3. Conclusiones sobre el Comportamiento de la Voz**

El análisis confirmó las diferencias anatómicas y fisiológicas esperadas entre voces masculinas y femeninas, evidenciando patrones consistentes en la producción vocal. Las voces femeninas se caracterizaron por un desplazamiento hacia frecuencias más altas, reflejado en una mayor frecuencia media ``+29.3%``) y un incremento en el brillo espectral (``+17.8%``). Este comportamiento se explica por la menor longitud del tracto vocal femenino (``14–15 cm`` frente a ``17–18 cm`` en hombres), lo que genera resonancias en frecuencias superiores. De acuerdo con los principios acústicos, un conducto más corto produce formantes más elevados, lo que eleva el centro de gravedad espectral y contribuye a la percepción de un timbre más agudo.

Los parámetros espectrales analizados (frecuencia media y brillo) mostraron consistencia y concordancia con lo descrito en la literatura, lo que confirma su utilidad como descriptores para diferenciar características vocales entre géneros.

En contraste, los parámetros temporales (frecuencia fundamental, jitter y shimmer) presentaron variaciones atípicas atribuibles a las limitaciones del procedimiento de cálculo utilizado. La frecuencia fundamental requiere técnicas de estimación que identifiquen de manera precisa la periodicidad de la señal, mientras que el método aplicado se basó en la detección del máximo espectral, el cual puede verse afectado por la energía de los armónicos. De manera similar, tanto el jitter como el shimmer mostraron valores anormalmente elevados (``22.80%`` y ``42.37%`` en hombres; ``20.11% y 40.98%``en mujeres respectivamente) que exceden ampliamente los rangos normales, evidenciando que no se identificaron correctamente los períodos glóticos. 

### **4. Importancia Clínica del Jitter y Shimmer**
>### 4.1 Relevancia Clínica

El jitter (variabilidad de frecuencia ciclo a ciclo) y shimmer (variabilidad de amplitud) son parámetros fundamentales en evaluación vocal objetiva. Miden la estabilidad de vibración de las cuerdas vocales.

>### Valores de referencia:

| Condición               | Jitter     | Shimmer  |
|-------------------------|------------|----------|
| Voz normal              | <1.0%      | <3–5%    |
| Patología leve-moderada | 1.0–5.0%   | 5–12%    |
| Patología severa        | >5.0%      | >12%     |

<br>

>[!NOTE]
>**Aplicaciones clínicas:** Diagnóstico de nódulos, pólipos, parálisis laríngea, edema de cuerdas vocales, y detección temprana de enfermedades neurodegenerativas como Parkinson. También útil en monitoreo de terapia vocal y recuperación postquirúrgica.

>### 4.2. Evaluación de Resultados Obtenidos

Los valores obtenidos para Jitter (``17–24%`` en todos los sujetos) y Shimmer (``37–50%`` en todos los sujetos, con promedios de ``42.37%`` en hombres y ``40.98%`` en mujeres) se encuentran muy por encima de los rangos fisiológicos reportados en la literatura (Jitter ``<1%``, Shimmer ``<3-5%``). Estos resultados no deben interpretarse como indicativos de patología vocal en los participantes, sino como consecuencia de limitaciones en el procedimiento de detección automática de ciclos glóticos. En particular, el uso de ``find_peaks()`` con una distancia mínima basada únicamente en la frecuencia estimada puede llevar a identificar oscilaciones de alta frecuencia o componentes armónicas en lugar de ciclos glóticos completos. La similitud de los valores anómalos entre ambos géneros confirma que se trata de un error del método aplicado.

>### 4.3. Utilidad en Ingeniería Biomédica
Más allá del diagnóstico clínico, jitter y shimmer tienen aplicaciones en telemedicina (monitoreo remoto de pacientes), sistemas de apoyo diagnóstico basados en inteligencia artificial, biometría vocal para autenticación, y como endpoints cuantificables en investigación farmacológica. Su medición no requiere equipamiento especializado, permitiendo evaluación objetiva mediante grabaciones simples, siempre que se utilicen algoritmos validados.

>[!NOTE]
>Para la comparación de los parámetros evaluados se tomaron como referencia los rangos reportados por Baken y Orlikoff (2000) en su obra ``"Clinical Measurement of Speech and Voice"``, texto estándar en la medición acústica de la voz que establece rangos típicos de frecuencia fundamental (F0) entre ``85-180 Hz`` para voces masculinas adultas y ``165-255 Hz`` para voces femeninas adultas, así como valores normativos para jitter (``<1.0%`` en voces normales) y shimmer (``<3-5%`` en voces normales). Estos rangos permitieron contextualizar los resultados obtenidos y evidenciar algunas discrepancias entre los valores medidos y los esperados según la fisiología vocal.

## **Notebook en *Google Colab***
Link: [Práctica 3 - Análisis Espectral de la Voz](https://colab.research.google.com/drive/1LQ8TNPYwzYItK29W52ATav75HM3xyUMV?usp=sharing)
