# EjercicioBlender
En Blender para crear alguna figura mediante python o comandos, se abre en el area de Scripts y a mano derecha es donde se colocara el texto. <br>
En este caso se creará un polígono 2D, por lo se importan las librerías y se crea la función de crear un polígono:
```python
import bpy
import math

```
1.La librería **bpy** nos servirá para crear la malla y el objeto. <br>
2.La librería **math** será para las funciones matemáticas que emplearemos para crear nuestro polígono. <br> 
```python
def crear_poligono_2d(nombre, lados, radio):
```
Esta función hará mi polígono 2D, donde dentro tendrá el nombre del objeto, su número de lados y la distancia de los vértices con el centro.<br>
Se creará una malla vacía y el objeto que usará esa malla, además de que se agregará a la colección que servirá para se vea:
```python
    malla = bpy.data.meshes.new(nombre)
    objeto = bpy.data.objects.new(nombre, malla)
    
    #vincular el objeto a la escena actual
    bpy.context.collection.objects.link(objeto)
```
Se crearán las variables o listas que guardarán los datos asignados.
```python
    vertices = []
    aristas = []
```
1. La lista de vértices será para guardar las coordenadas (x,y,z). <br>
2. La lista de aristas guardará cuáles son los vértices que se conectan entre sí. <br>
Ahora se deben hacer los cálculos de los vértices y definir las conexiones entre los vértices:
```python
    #Calculo de vertices usando coordenadas polares a cartesianas
    for i in range(lados):
        angulos = 2 * math.pi * i / lados
        x = radio * math.cos(angulos)
        y = radio * math.sin(angulos)
        vertices.append((x, y, 0)) #Z=0 para mantenerlo en 2D
        
    #definir las conexiones (aristas) entre los vertices
    for i in range(lados):
        aristas.append((i,(i + 1) % lados))
```
- Vértices: Se calculan al usar las coordenadas polares a cartesianes empleando la función de cos para x y sin para y, la parte del z donde está "0" se coloca pues queremos una figura 2D. La circunferencia completa se divide entre el numero de lados para obtener los angulos que se utilizan en el cálculo de x y y. <br>
- Aristas: Hará que cada vértice se conecte con el siguiente, empleando lo siguiente: <br>
  1.- `(i + 1)` conecta con el siguiente punto. <br>
  2.- `% lados` hace que el último vértice se conecte con el primero para lograr cerrar la figura. <br>
Se tienen que mandar los datos anteriores a la malla, por lo que se agrega lo siguiente:
  ```python
      malla.from_pydata(vertices, aristas, [])
      malla.update()
  ```
- `malla.from_pydata()`: Recibirá el valor de los vértices y las conexiones entre estos, el **[]** es para señalar que esta vacío, es decir, sin superficie.<br>
- `malla.update()`: Va actualizar la malla.<br>
Por consiguiente, se tiene que limpiar la escena pues debe eliminar los objetos anteriores para mostrar el polígono generado:
```python
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()
```
Y finalmente, se llama a la función generada anteriormente y asignando sus valores:
```python
crear_poligono_2d("Poligono2D", lados=6, radio=5) 
```
Lo que generará un hexágono con un radio de 5, asi es como se ve: <br>
<img width="894" height="706" alt="image" src="https://github.com/user-attachments/assets/71eb6a1c-2eb3-475f-afa1-9a06abee1d43" />
# Código completo
```python
import bpy
import math

def crear_poligono_2d(nombre, lados, radio):
    #crear una nueva malla y un nuevo objeto
    malla = bpy.data.meshes.new(nombre)
    objeto = bpy.data.objects.new(nombre, malla)
    
    #vincular el objeto a la escena actual
    bpy.context.collection.objects.link(objeto)
    
    vertices = []
    aristas = []
    
    #Calculo de vertices usando coordenadas polares a cartesianas
    for i in range(lados):
        angulos = 2 * math.pi * i / lados
        x = radio * math.cos(angulos)
        y = radio * math.sin(angulos)
        vertices.append((x, y, 0)) #Z=0 para mantenerlo en 2D
        
    #definir las conexiones (aristas) entre los vertices
    for i in range(lados):
        aristas.append((i,(i + 1) % lados))
            
    #cargar los datos en la malla
    malla.from_pydata(vertices, aristas, [])
    malla.update()
    
#limpiar la escena antes de empezar
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()
    
#llamada a la funcion: un hexagono de radio 5
crear_poligono_2d("Poligono2D", lados=6, radio=5)    
 ```

