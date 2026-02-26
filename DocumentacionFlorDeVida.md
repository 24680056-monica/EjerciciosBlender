## Flor De vida
Aca se realizará la documentación y explicación del desarrollo de la práctica de hacer una flor de vida.<br>
Inicialmente se importan las librerías necesarias. 

```python
import bpy
import math
```
- `bpy` : Es la librería de Blender para manipular objetos.
- `math` : Permite usar funciones matemáticas como seno y coseno.

``` python
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()
```
Se debe limpiar ahora la escena, con estos comandos los seleccionará y posteriormente los eliminará.
``` python
radio = 3
angulo_actual = 0
paso_angular = 60
```
- `radio = 3`  Radio de cada círculo.
- `angulo_actual` = 0  Ángulo inicial.
- `paso_angular` = 60  Cada cuánto gira el ángulo. Esto nos señala que habrá 6 círculos alrededor del central.

``` python
bpy.ops.mesh.primitive_circle_add(radius=radio, location=(0, 0, 0), vertices=64)
```
Este es para crear el circulo central ubicado al centro y los vertices son para hacerlo mas suave. <br>

Ahora se inicia un bucle para evitar hacerlo manualmente y asi crear 6 circulos alrededor del central.
```python
angulo_actual = 0
while angulo_actual <360:
```
El while que se ejecutará hasta que el ángulo llegue a 360°.

``` python
    angulo_actual += paso_angular
```
Este va ir sumando 60 hasta llegar a 360
``` python
    x = radio * math.cos(math.radians(angulo_actual))
    y = radio * math.sin(math.radians(angulo_actual))
```
En esta partse se está usando coordenadas polares: 
- 𝑥 = 𝑟cos(𝜃)
- 𝑦 = 𝑟sin(𝜃)

- `math.radians()` : convierte grados a radianes (porque cos y sin usan radianes).
- Multiplica por `radio` para que los círculos queden justo tocando el central.

```python
    bpy.ops.mesh.primitive_circle_add(radius=radio, location=(x, y, 0), vertices=64)
```
Esta funcion crea un nuevo círculo en: (x, y, 0) con mismo radio y mismos vértices.

### Visualización
<img width="622" height="523" alt="image" src="https://github.com/user-attachments/assets/c0fcee40-d5d9-430f-9c53-b328e1c96c69" />

## Código completo
```python
import bpy
import math

#Limpiar escena
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()

#Parámetros de la figura
radio = 3
angulo_actual = 0
paso_angular = 60 #Cada 60 grados para obtener 6 circulos alrededor

#1.Circulo Central
bpy.ops.mesh.primitive_circle_add(radius=radio, location=(0, 0, 0), vertices=64)

#...INICIO DEL PATRÓN REPETITIVO

#bucle
angulo_actual = 0
while angulo_actual <360:

    angulo_actual += paso_angular
    
    x = radio * math.cos(math.radians(angulo_actual))
    y = radio * math.sin(math.radians(angulo_actual))
    bpy.ops.mesh.primitive_circle_add(radius=radio, location=(x, y, 0), vertices=64)
```
