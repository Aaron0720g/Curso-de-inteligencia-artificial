# Ejercicio 1 — Greedy vs A\* en el mapa de Rumania

**Pareja elegida:** Oradea → Bucharest
1. Subgrafo usado

Entre paréntesis está `h(n)` hacia Bucharest; sobre las aristas, los km.

```
                        Zerind (374)
                           |
                           | 71
                           |
   origen ->            Oradea (380)
                           |
                           | 151
                           |
      Arad (366) --140-- Sibiu (253) --99-- Fagaras (176)
                           |                   |
                           | 80                | 211
                           |                   |
  Craiova (160) --146-- Rimnicu Vilcea (193)   |
     |                     |                   |
     | 138                 | 97                |
     |                     |                   |
     +----------------- Pitesti (100)          |
                           |                   |
                           | 101               |
                           |                   |
                           +-- Bucharest (0) --+
                                 destino
```

Camino Greedy: Oradea - Sibiu - Fagaras - Bucharest
Camino A\*: Oradea - Sibiu - Rimnicu Vilcea - Pitesti - Bucharest

## 2\. Tabla comparativa

||Greedy best-first|A\*|
|-|-|-|
|Heurística|línea recta a Bucharest (AIMA)|línea recta a Bucharest (AIMA)|
|Path|Oradea → Sibiu → Fagaras → Bucharest|Oradea → Sibiu → Rimnicu Vilcea → Pitesti → Bucharest|
|Depth|3 carreteras|4 carreteras|
|Cost|461 km|429 km|
|Expanded|3 nodos|5 nodos|
|Generated|9 nodos|15 nodos|
|Frontier máx.|4|5|

### g / h / f a lo largo de cada camino

Greedy:

|ciudad|g|h|f|
|-|-|-|-|
|Oradea|0|380|380|
|Sibiu|151|253|404|
|Fagaras|250|176|426|
|Bucharest|461|0|461|

A\*:

|ciudad|g|h|f|
|-|-|-|-|
|Oradea|0|380|380|
|Sibiu|151|253|404|
|Rimnicu Vilcea|231|193|424|
|Pitesti|328|100|428|
|Bucharest|429|0|429|

## 3\. Reporte

### ¿A\* encontró el camino de menos km? ¿Greedy coincidió o se desvió?

Sí, A\* encontró el de menos kilómetros: Oradea - Sibiu - Rimnicu Vilcea - Pitesti - Bucharest, 429 km. Greedy no coincidió, se desvió y terminó en 461 km, 32 km más.

Los dos arrancan igual, porque desde Oradea solo hay dos vecinos, Sibiu (h = 253) y Zerind (h = 374), y ambos bajan por Sibiu. La discrepancia aparece justo en Sibiu. Greedy solo mira h(n): compara Fagaras (h = 176) contra Rimnicu Vilcea (h = 193) y se queda con Fagaras porque se ve más cerca de Bucharest en línea recta. A\* compara f(n) = g(n) + h(n) en ese mismo punto: para Fagaras f = 250 + 176 = 426, para Rimnicu Vilcea f = 231 + 193 = 424, y gana Rimnicu Vilcea por 2 km de estimación. El desvío de Greedy se paga con el tramo Fagaras - Bucharest, que son 211 km, el más caro de la zona.

### ¿Por qué Greedy puede devolver un camino más caro aunque h sea admisible?

Porque la admisibilidad solo garantiza que la estimación no se pasa del costo real que falta; no dice nada sobre lo que ya se gastó. Greedy ignora g(n) por completo, así que no puede notar que un nodo con mejor h está detrás de una carretera larga. La admisibilidad sirve para la optimalidad de A\*, no para la de Greedy.

### En el camino de A\*, ¿f tiende a no disminuir a lo largo de la ruta?

Sí. En el camino de A\*, f va 380, 404, 424, 428, 429, y nunca baja. Eso es lo que se espera de una heurística consistente, que cumple h(n) ≤ c(n, n') + h(n'); al sumar g, cada paso hace que f se mantenga o suba. Aquí el destino es Bucharest y se usa la tabla AIMA de distancias en línea recta, que es consistente, así que la primera vez que A\* saca Bucharest de la frontera ya trae el costo óptimo.

### ¿Cuál algoritmo trabajó más?

A\*. Expandió 5 nodos y generó 15, contra 3 y 9 de Greedy. Greedy trabajó menos, pero entregó un camino 32 km peor. A\* paga esos nodos extra por explorar Rimnicu Vilcea y Pitesti, y a cambio garantiza el óptimo.

## 4\. Evidencias

### `python 02\_heuristics.py --from-city Oradea --to Bucharest`

```
Heuristic: straight-line distance to Bucharest (AIMA table)

  h(n)  city
      0  Bucharest  <- goal
     77  Giurgiu
     80  Urziceni
    100  Pitesti
    151  Hirsova
    160  Craiova
    161  Eforie
    176  Fagaras
    193  Rimnicu Vilcea
    199  Vaslui
    226  Iasi
    234  Neamt
    241  Mehadia
    242  Drobeta
    244  Lugoj
    253  Sibiu
    329  Timisoara
    366  Arad
    374  Zerind
    380  Oradea  <- start
```

### `python 03\_greedy\_best\_first\_search.py --from-city Oradea --to Bucharest`

```
Algorithm: Greedy best-first search
Problem:   Oradea → Bucharest
Heuristic: straight-line distance to Bucharest (AIMA table)
Status:    success
Path:      Oradea → Sibiu → Fagaras → Bucharest
Depth:     3 roads
Cost:      461 km

  city                  g     h     f
  Oradea                   0   380   380
  Sibiu                  151   253   404
  Fagaras                250   176   426
  Bucharest              461     0   461

Expanded:  3 nodes
Generated: 9 nodes
Frontier:  max size 4
```

### `python 04\_a\_star\_search.py --from-city Oradea --to Bucharest`

```
Algorithm: A\* search
Problem:   Oradea → Bucharest
Heuristic: straight-line distance to Bucharest (AIMA table)
Status:    success
Path:      Oradea → Sibiu → Rimnicu Vilcea → Pitesti → Bucharest
Depth:     4 roads
Cost:      429 km

  city                  g     h     f
  Oradea                   0   380   380
  Sibiu                  151   253   404
  Rimnicu Vilcea         231   193   424
  Pitesti                328   100   428
  Bucharest              429     0   429

Expanded:  5 nodes
Generated: 15 nodes
Frontier:  max size 5
```

### `python 01\_romania\_map.py --from-city Oradea`

```
Romania road map (AIMA Figure 3.2)
Cities: 20   Roads: 23

  Oradea: Sibiu 151 km, Zerind 71 km
```

