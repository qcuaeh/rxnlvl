irxnlvl
======

`irxnlvl` es un programa para dibujar fácilmente diagramas de niveles de energía de reacciones químicas. Es una versión modificada de [rxnvl](https://github.com/eutactic/rxnlvl) que puede ejecutarse interactivamente en Jupyter Notebook.

![diagrama 2](diagrama2.png)

¿Qué necesito?
------
Necesitas clonar o descargar `irxnlvl` y tener instalado Python 3.4 o superior o puedes probar `irxnlvl` en Binder sin necesidad de instalar nada en tu computadora.

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/qcuaeh/irxnlvl.git/HEAD)

¿Cómo lo uso?
------
Para usarlo se requiere escribir código Python, pero incluso si no sabes python deberías poder crear diagramas fácilmente. Para aprender a crear tus primeros diagramas puedes ejecutar y modificar los siguientes ejemplos en Binder:

- [Ejemplo 1](https://mybinder.org/v2/gh/qcuaeh/irxnlvl.git/HEAD?labpath=example1.ipynb)
- [Ejemplo 2](https://mybinder.org/v2/gh/qcuaeh/irxnlvl.git/HEAD?labpath=example2.ipynb)

Los pasos utilizados en los ejemplos se explican a continuación.

### Importar el módulo

    from irxnlvl import *

### Crear un diagrama vacío para poder agregar elementos

    p = plot(10.0, vbuf=10.0, hbuf=5.0, bgcolour=None, zero=energy(0.0, 'kjmol'), units='kjmol', digits=1)
    
`plot` toma los siguientes argumentos:
- `size` - El tamaño vertical del gráfico en cm.
- `bgcolour` - el color de fondo de la imagen, como un entero hexadecimal de 24 bits, o `None`. Si `None`, el fondo será transparente.
- `zero` - un objeto `energy` que representa la energía de referncia que será el cero de las energías relativas. La energía tiene dos argumentos - la energía como un número de punto flotante, y las unidades, que pueden ser `'kjmol'`, `'eh'` (Hartrees), `'ev'` (electronvoltios), `'kcalmol'` (kilocalorías por mol termoquímicas) o `'wavenumber'`.
- `units` - Las unidades de energía del diagrama, que pueden ser `'kjmol'`, `'eh'` (Hartrees), `'ev'` (electronvoltios), `'kcalmol'` (kilocalorías por mol termoquímicas) o `'wavenumber'`.
- `digits` - Los dígitos después del punto decimal que se usarán para mostrar las energías del diagrama.
- `qualified` - Si es `True`, las unidades en las que cada energía es especificada serán impresas en la imagen. Si es `False`, sólo imprimirá los valores numéricos. Si se especifica *cualquier* valor de cadena, sólo imprimirá las unidades en el nivel de energía de extrema izquierda, que es útil cuando quieres dar las unidades en tu gráfica pero no quieres atiborrarla.


Ahora podemos empezar a agregar elementos al gráfico.

### Agregar una línea base

    p + baseline(colour=0x0, mode='dashed', opacity=0.1)

Sólo puedes tener una línea de base que representa el cero de las energías relativas y toma los siguientes argumentos:
- `colour` - un entero hexadecimal de 24 bits representando el color de la arista.
- `mode` - elije entre `'normal'` o `'dashed'`. Controla la apariencia de la arista en términos de la discontinuidad de la línea.
- `opacity` - un flotante entre 0.0 y 1.0 representando la opacidad de la arista.

### Agregar niveles de energía

    p +  level(energy(0, 'kjmol'),  1,  '1',  0x0)

Cada objeto `level` toma los siguientes argumentos:
- `energy` - un objeto `energy` que representa la energía relativa del nivel. Cada energía tiene dos argumentos - la energía como un número de punto flotante, y las unidades, que pueden ser `'kjmol'`, `'eh'` (Hartrees), `'ev'` (electronvoltios), `'kcalmol'` (kilocalorías por mol termoquímicas) o `'wavenumber'`.
- `location` - la ubicación ordinal del nivel en el esquema. Éste debe ser un entero positivo diferente de cero. Diferentes niveles pueden compartir la misma ubicación.
- `name` - el nombre del nivel en el esquema. Los niveles no deberían compartir el mismo nombre.
- `colour` - un entero hexadecimal de 24 bits representando el color del nivel.

### Unir los niveles de energía

    p +  edge(    '1',  'EC1', 0x0, 0.4, 'normal')

Cada `edge` toma los siguientes argumentos:
- `start` - el `name` del nivel del que se origina la arista.
- `end` - el `name` del nivel en el que termina la arista. Éste tiene que ser diferente de `start`.
- `colour` - un entero hexadecimal de 24 bits representando el color de la arista.
- `opacity` - un flotante entre 0.0 y 1.0 representando la opacidad de la arista.
- `mode` - elije entre `'normal'` o `'dashed'`. Controla la apariencia de la arista en términos de la discontinuidad de la línea.

### Visualizar el diagrama

    p.render()

### Y guardarlo

    p.write('diagrama.svg')

Creará un archivo `diagrama.svg` de tu gráfica en la misma carpeta donde se abrió el Notebook.
