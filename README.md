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







