
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

| **Sujeto**          | **F0 (Hz)** | **F Media (Hz)** | **Brillo** | **Energía (×10¹⁵)** | **Jitter (%)** |
|-----------------|---------|--------------|--------|-----------------|------------|
| Hombre 1        | 577.18  | 3142.40      | 0.488  | 82.65           | 21.14      |
| Hombre 2        | 571.95  | 3080.58      | 0.439  | 106.80          | 23.12      |
| Hombre 3        | 499.36  | 4487.85      | 0.474  | 86.68           | 24.14      |
| Mujer 1         | 246.41  | 3972.60      | 0.469  | 44.94           | 21.30      |
| Mujer 2         | 446.49  | 4866.95      | 0.630  | 63.99           | 17.81      |
| Mujer 3         | 422.92  | 5008.83      | 0.550  | 64.36           | 21.21      |
| **Promedio Hombres** | **549.50** | **3570.28** | **0.467** | **92.04** | **22.80** |
| **Promedio Mujeres** | **371.94** | **4616.13** | **0.550** | **57.76** | **20.11** |


>### 2.2 Gráficos Comparativos

>### Gráfico 1:

<img width="806" height="343" alt="image" src="https://github.com/user-attachments/assets/23863567-4e4e-4f88-9155-8929cb52703f" />

El gráfico de frecuencia fundamental muestra valores considerablemente más altos en el grupo masculino (promedio ``549.5 Hz``, rango ``499-577 Hz``) comparado con el grupo femenino (promedio ``371.9 Hz``, rango ``246-446 Hz``). Sin embargo, estos valores son fisiológicamente anómalos, debido a que en condiciones normales las voces masculinas deberían presentar F0 entre ``85-180 Hz`` y las femeninas entre ``165-255 Hz``. La inversión observada (hombres con valores más altos que mujeres) confirma que el algoritmo está detectando armónicos de alta energía en lugar de la frecuencia fundamental real, particularmente pronunciado en las muestras masculinas donde los armónicos superiores tienen mayor amplitud espectral.

>### Gráfico 2:

<img width="799" height="349" alt="image" src="https://github.com/user-attachments/assets/a7d5278d-89d2-478a-9cd2-782aa3f71c17" />

La frecuencia media muestra una clara diferenciación entre grupos: las mujeres presentan valores consistentemente más altos (promedio ``4,616 Hz``, rango ``3,973-5,009 Hz``) que los hombres (promedio ``3,570 Hz``, rango ``3,081-4,488 Hz``). Este resultado es coherente con las diferencias anatómicas del tracto vocal, donde la menor longitud en mujeres desplaza el centro de gravedad espectral hacia frecuencias superiores. La dispersión dentro de cada grupo es moderada, con el Hombre 3 mostrando valores atípicamente elevados que se acercan al rango femenino, posiblemente debido a características individuales de su configuración vocal.

>### Gráfico 3:

<img width="803" height="342" alt="image" src="https://github.com/user-attachments/assets/ef0979ed-5a0a-48de-8fdc-d1c978dc78ee" />

El brillo espectral evidencia mayor concentración de energía en altas frecuencias para el grupo femenino (promedio ``0.550``, rango ``0.469-0.630``) comparado con el masculino (promedio ``0.467``, rango ``0.439-0.488``). Los datos muestran separación clara entre grupos con solapamiento mínimo, indicando que este parámetro es un buen discriminador entre voces masculinas y femeninas. La Mujer 2 presenta el valor más alto de brillo (0.630), sugiriendo una voz particularmente aguda o con mayor contenido armónico en el rango superior a ``1500 Hz``, mientras que el Hombre 2 muestra el menor brillo (``0.439``), caracterizando una voz más "opaca" o con predominancia de frecuencias bajas.

>### Gráfico 4:

<img width="802" height="355" alt="image" src="https://github.com/user-attachments/assets/60ac8227-0344-41e1-8179-e567434de9af" />

La energía total revela mayor intensidad en las grabaciones masculinas (promedio ``92.04×10¹⁵``, rango ``82.65-106.8×10¹⁵``) en comparación con las femeninas (promedio ``57.76×10¹⁵``, rango ``44.94-64.36×10¹⁵``), con una diferencia aproximada del 38%. La considerable variabilidad observada, especialmente en el grupo masculino donde el Hombre 2 alcanza valores significativamente superiores, sugiere que este parámetro está influenciado tanto por factores fisiológicos (capacidad de presión subglótica, masa de cuerdas vocales) como por variables metodológicas no controladas (distancia al micrófono, ganancia de grabación, volumen de fonación).

>### Gráfico 5:

<img width="811" height="361" alt="image" src="https://github.com/user-attachments/assets/e7679297-679a-4b4d-be29-bb0075bb88ac" />

El jitter relativo muestra valores similares entre ambos grupos, con promedios de ``22.80%`` en hombres (rango ``21.14-24.14%``) y ``20.11%`` en mujeres (rango ``17.81-21.30%``). La distribución homogénea y la ausencia de diferencias significativas entre géneros contrasta con las expectativas fisiológicas, pero más importante aún, los valores absolutos exceden dramáticamente los rangos clínicos normales (``<1%``) e incluso patológicos severos (``>5%``). Esta consistencia en valores anormalmente elevados en todos los participantes confirma un error sistemático en el algoritmo de detección de ciclos glóticos, independiente del género del hablante, indicando que la metodología requiere corrección antes de que estos resultados puedan considerarse válidos clínicamente.

> [!IMPORTANT]  
> Los valores de F0 y Jitter presentan anomalías significativas. F0 esperado: ``5-180 Hz`` (hombres), ``165-255 Hz`` (mujeres). Jitter normal: ``<1.5%``

>### 2.3 Diferencias Observadas en Frecuencia Fundamental

| Grupo   | Rango de Referencia Literatura | Valor Obtenido | Observación                    |
|---------|--------------------------------|----------------|--------------------------------|
| Hombres | 85–180 Hz                      | 549.5 Hz       | Valor superior al rango típico |
| Mujeres | 165–255 Hz                     | 371.9 Hz       | Valor superior al rango típico |

>[!NOTE]
>Los valores obtenidos se encuentran por encima de los rangos reportados en la literatura para voces adultas. El método de detección basado en máximo de FFT puede ser sensible a componentes armónicos de mayor amplitud en el espectro, lo que puede influir en los valores registrados.

>### 2.4 Diferencias en términos de Brillo, Frecuencia Media e Intensidad

**Frecuencia Media**: Las mujeres presentaron valores 29.3% mayores (``4,616 Hz`` vs ``3,570 Hz``), reflejando el desplazamiento espectral hacia frecuencias altas debido a un tracto vocal más corto (``~15 cm`` vs ``~17 cm`` en hombres). Este resultado es consistente con la fisiología vocal.

**Brillo Espectral**: Las mujeres mostraron ``17.8%`` más brillo (``0.550`` vs ``0.467``), indicando mayor concentración de energía por encima de ``1500 Hz``. Esto explica perceptualmente por qué las voces femeninas suenan más "agudas" o "brillantes". Este resultado también es consistente y esperado.

**Intensidad**: Los hombres registraron 38% mayor energía. Aunque puede reflejar diferencias fisiológicas (mayor masa de cuerdas vocales), sin normalización de volumen no es posible distinguir entre diferencias reales y artefactos de grabación (distancia al micrófono, ganancia variable).

### **3. Conclusiones sobre el Comportamiento de la Voz**

El análisis espectral confirmó las diferencias anatómicas y fisiológicas esperadas entre voces masculinas y femeninas. Las voces femeninas exhiben desplazamiento sistemático hacia frecuencias más altas, manifestado en mayor frecuencia media (``+29.3%``) y brillo espectral (``+17.8%``). Esto se origina en la menor longitud del tracto vocal femenino, que genera resonancias (formantes) en posiciones frecuenciales superiores según la teoría acústica de tubos.
Los parámetros espectrales (frecuencia media y brillo) fueron consistentes y válidos, mientras que las mediciones temporales (F0 y jitter) presentaron valores anómalos que indican limitaciones en los algoritmos utilizados. Es importante destacar que la frecuencia fundamental requiere métodos especializados como autocorrelación.

### **4. Importancia Clínica del Jitter y Shimmer**
>### 4.1 Relevancia Clínica

El jitter (variabilidad de frecuencia ciclo a ciclo) y shimmer (variabilidad de amplitud) son parámetros fundamentales en evaluación vocal objetiva. Miden la estabilidad de vibración de las cuerdas vocales.

>### Valores de referencia:

| Condición               | Jitter     | Shimmer  |
|-------------------------|------------|----------|
| Voz normal              | <1.0%      | <3–5%    |
| Patología leve-moderada | 1.0–5.0%   | 5–12%    |
| Patología severa        | >5.0%      | >12%     |

>[!NOTE]
>**Aplicaciones clínicas:** Diagnóstico de nódulos, pólipos, parálisis laríngea, edema de cuerdas vocales, y detección temprana de enfermedades neurodegenerativas como Parkinson. También útil en monitoreo de terapia vocal y recuperación postquirúrgica.

>### 4.2. Evaluación de Resultados Obtenidos

Los valores obtenidos para Jitter (``17–24%`` en todos los sujetos) y Shimmer (``37–42%`` en hombres) se encuentran muy por encima de los rangos fisiológicos reportados en la literatura. Estos resultados no deben interpretarse como indicativos de patología vocal en los participantes, sino como consecuencia de limitaciones en el procedimiento de detección automática de ciclos glóticos. En particular, el uso de ``find_peaks()`` con una distancia mínima basada únicamente en la frecuencia estimada puede llevar a identificar oscilaciones de alta frecuencia en lugar de ciclos glóticos completos.

>### 4.3. Utilidad en Ingeniería Biomédica
Más allá del diagnóstico clínico, jitter y shimmer tienen aplicaciones en telemedicina (monitoreo remoto de pacientes), sistemas de apoyo diagnóstico basados en inteligencia artificial, biometría vocal para autenticación, y como endpoints cuantificables en investigación farmacológica. Su medición no requiere equipamiento especializado, permitiendo evaluación objetiva mediante grabaciones simples, siempre que se utilicen algoritmos validados.

>[!NOTE]
>Para la comparación de los parámetros evaluados se tomaron como referencia los rangos reportados por Baken y Orlikoff (2000) en su obra ``"Clinical Measurement of Speech and Voice"``, texto estándar en la medición acústica de la voz que establece rangos típicos de frecuencia fundamental (F0) entre ``85-180 Hz`` para voces masculinas adultas y ``165-255 Hz`` para voces femeninas adultas, así como valores normativos para jitter (``<1.0%`` en voces normales) y shimmer (``<3-5%`` en voces normales). Estos rangos permitieron contextualizar los resultados obtenidos y evidenciar algunas discrepancias entre los valores medidos y los esperados según la fisiología vocal.







