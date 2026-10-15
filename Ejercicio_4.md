# Ejercicio 1 — BFS, UCS, DFS, DLS e IDS en el mapa de Rumania

**Pareja elegida:** Oradea → Bucharest

## 1\. Subgrafo usado

Incluye las ciudades que aparecen en los caminos de los cinco algoritmos. Los números son kilómetros.

```
 Zerind ----75----- Arad ---118---- Timisoara ---------111--------- Lugoj
    |                 |                                               |
    | 71              | 140                                           | 70
    |                 |                                               |
 Oradea ----151---- Sibiu                                          Mehadia
                      |                                               |
           +----99----+----80----+                                    | 75
           |                     |                                    |
        Fagaras            Rimnicu Vilcea --146--- Craiova --120-- Drobeta
           |                     |                    |
           | 211                 | 97                 | 138
           |                     |                    |
           |                  Pitesti ----------------+
           |                     |
           |                     | 101
           |                     |
           +----- Bucharest -----+
                  destino
```

Caminos obtenidos:

* BFS, DLS(3), DLS(4), IDS: Oradea - Sibiu - Fagaras - Bucharest
* UCS: Oradea - Sibiu - Rimnicu Vilcea - Pitesti - Bucharest
* DFS: Oradea - Sibiu - Arad - Timisoara - Lugoj - Mehadia - Drobeta - Craiova - Pitesti - Bucharest

## 2\. Tabla comparativa

|Algoritmo|Status|Path|Depth|Cost|Expanded|Generated|
|-|-|-|-|-|-|-|
|BFS|success|Oradea → Sibiu → Fagaras → Bucharest|3|461 km|5|13|
|UCS|success|Oradea → Sibiu → Rimnicu Vilcea → Pitesti → Bucharest|4|429 km|10|27|
|DFS|success|Oradea → Sibiu → Arad → Timisoara → Lugoj → Mehadia → Drobeta → Craiova → Pitesti → Bucharest|9|1024 km|9|24|
|DLS `--limit 2`|cutoff|—|—|—|3|9|
|DLS `--limit 3`|success|Oradea → Sibiu → Fagaras → Bucharest|3|461 km|4|8|
|DLS `--limit 4`|success|Oradea → Sibiu → Fagaras → Bucharest|3|461 km|6|12|
|IDS|success|Oradea → Sibiu → Fagaras → Bucharest|3|461 km|8|21|

## 3\. Reporte

### ¿BFS encontró el camino con menos carreteras? ¿UCS el de menos km?

Sí a las dos. BFS devolvió Oradea - Sibiu - Fagaras - Bucharest, 3 tramos, que es el mínimo posible. UCS devolvió Oradea - Sibiu - Rimnicu Vilcea - Pitesti - Bucharest, 429 km, que es el mínimo en kilómetros.

Los dos caminos no coinciden, y ese es el punto interesante de esta pareja: el camino más corto en número de carreteras cuesta 461 km porque el tramo Fagaras - Bucharest son 211 km, mientras que el camino barato mete una ciudad más (Pitesti) pero suma tramos chicos, 151 + 80 + 97 + 101. BFS ordena la frontera por profundidad y no mira los kilómetros, así que devuelve el primer camino de 3 saltos que alcanza; UCS ordena por costo acumulado y por eso se va por Rimnicu Vilcea.

### ¿Por qué DFS puede devolver un camino más largo aunque el grafo sea el mismo?

Porque su frontera es una pila: baja hasta el fondo de la primera rama que encuentra antes de considerar alternativas. Desde Sibiu el primer vecino en orden alfabético es Arad, así que se mete a Arad - Timisoara - Lugoj - Mehadia - Drobeta - Craiova y solo llega a Bucharest por Pitesti después de rodear medio mapa: 9 carreteras y 1024 km, más del doble que UCS. El conjunto de explorados evita que se cicle, pero nada le impide devolver la primera solución que topa, que no tiene por qué ser buena ni en saltos ni en kilómetros.

### ¿Con qué --limit DLS pasó de cutoff a solución, y cómo se relaciona con la profundidad de BFS/IDS?

Con `--limit 2` dio `cutoff`, y con `--limit 3` encontró solución. El salto ocurre exactamente en 3, que es la profundidad del camino que devuelven BFS e IDS. Tiene sentido: no existe ningún camino de Oradea a Bucharest en 2 carreteras o menos, así que por debajo de 3 el límite corta antes de llegar, y a partir de 3 ya alcanza la solución más superficial. Con `--limit 4` devuelve el mismo camino de 3 carreteras, solo que expande más nodos porque explora un nivel extra.

### ¿IDS coincide con BFS en profundidad?

Sí, y también en el camino: los dos devuelven Oradea - Sibiu - Fagaras - Bucharest con 3 carreteras. Es lo esperado, porque IDS repite DLS con límites 0, 1, 2, 3 y se queda con la primera solución, que por construcción es la más superficial. El precio es reexpandir los niveles de arriba en cada iteración: 8 nodos expandidos y 21 generados contra 5 y 13 de BFS, a cambio de usar mucha menos memoria.

### ¿Cuál algoritmo trabajó más?

UCS, con 10 nodos expandidos y 27 generados, seguido de DFS con 9 e IDS con 8. DLS con límite 3 fue el más barato con 4 nodos, pero solo porque le dimos el límite correcto de antemano, que es información que normalmente no se tiene.

## 4\. Evidencias

### `python 01\_romania\_map.py --from-city Oradea`

```
Romania road map (AIMA Figure 3.2)
Cities: 20   Roads: 23

  Oradea: Sibiu 151 km, Zerind 71 km
```

### `python 02\_breadth\_first\_search.py --from-city Oradea --to Bucharest`

```
Algorithm: Breadth-first search
Problem:   Oradea → Bucharest
Status:    success
Path:      Oradea → Sibiu → Fagaras → Bucharest
Depth:     3 roads
Cost:      461 km
Expanded:  5 nodes
Generated: 13 nodes
Frontier:  max size 4
```

### `python 03\_uniform\_cost\_search.py --from-city Oradea --to Bucharest`

```
Algorithm: Uniform-cost search
Problem:   Oradea → Bucharest
Status:    success
Path:      Oradea → Sibiu → Rimnicu Vilcea → Pitesti → Bucharest
Depth:     4 roads
Cost:      429 km
Expanded:  10 nodes
Generated: 27 nodes
Frontier:  max size 4
```

### `python 04\_depth\_first\_search.py --from-city Oradea --to Bucharest`

```
Algorithm: Depth-first search
Problem:   Oradea → Bucharest
Status:    success
Path:      Oradea → Sibiu → Arad → Timisoara → Lugoj → Mehadia → Drobeta → Craiova → Pitesti → Bucharest
Depth:     9 roads
Cost:      1024 km
Expanded:  9 nodes
Generated: 24 nodes
Frontier:  max size 4
```

### `python 05\_depth\_limited\_search.py --from-city Oradea --to Bucharest --limit 2`

```
Algorithm: Depth-limited search
Problem:   Oradea → Bucharest
Status:    cutoff
Detail:    limit=2
Expanded:  3 nodes
Generated: 9 nodes
Frontier:  max size 6
```

### `python 05\_depth\_limited\_search.py --from-city Oradea --to Bucharest --limit 3`

```
Algorithm: Depth-limited search
Problem:   Oradea → Bucharest
Status:    success
Detail:    limit=3
Path:      Oradea → Sibiu → Fagaras → Bucharest
Depth:     3 roads
Cost:      461 km
Expanded:  4 nodes
Generated: 8 nodes
Frontier:  max size 6
```

### `python 05\_depth\_limited\_search.py --from-city Oradea --to Bucharest --limit 4`

```
Algorithm: Depth-limited search
Problem:   Oradea → Bucharest
Status:    success
Detail:    limit=4
Path:      Oradea → Sibiu → Fagaras → Bucharest
Depth:     3 roads
Cost:      461 km
Expanded:  6 nodes
Generated: 12 nodes
Frontier:  max size 6
```

### `python 06\_iterative\_deepening\_search.py --from-city Oradea --to Bucharest`

```
Algorithm: Iterative deepening search
Problem:   Oradea → Bucharest
Status:    success
Detail:    last\\\_limit=3
Path:      Oradea → Sibiu → Fagaras → Bucharest
Depth:     3 roads
Cost:      461 km
Expanded:  8 nodes
Generated: 21 nodes
Frontier:  max size 6
```

