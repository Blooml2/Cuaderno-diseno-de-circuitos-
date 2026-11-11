<p align="center"><a href="README.md"><img src="assets/tab-README-off.svg" alt="Inicio" height="40"></a> <a href="semana-1.md"><img src="assets/tab-semana-1-off.svg" alt="Semana 1" height="40"></a> <a href="semana-2.md"><img src="assets/tab-semana-2-off.svg" alt="Semana 2" height="40"></a> <a href="semana-3.md"><img src="assets/tab-semana-3-on.svg" alt="Semana 3" height="40"></a> <a href="semana-4.md"><img src="assets/tab-semana-4-off.svg" alt="Semana 4" height="40"></a> <a href="glosario.md"><img src="assets/tab-glosario-off.svg" alt="Glosario" height="40"></a></p>

<p align="center"><img src="assets/banner-semana-3.svg" alt="Semana 3: Mapas de Karnaugh y universalidad NAND/NOR" width="100%"></p>

Un mapa de Karnaugh es la tabla de verdad acomodada en una cuadrícula de modo que **las celdas vecinas difieren en un solo bit**. Así la simplificación se vuelve visible.

## 1. Construcción del mapa

> [!NOTE]
> Las filas y columnas se ordenan en **código Gray**: 00, 01, 11, 10. Por eso el mapa es "circular": la primera fila es vecina de la última, y la primera columna de la última.

**Mapa de 3 variables (8 celdas)**

| A \ BC | 00 | 01 | 11 | 10 |
|:-:|:-:|:-:|:-:|:-:|
| **0** | m0 | m1 | m3 | m2 |
| **1** | m4 | m5 | m7 | m6 |

**Mapa de 4 variables (16 celdas)**

| AB \ CD | 00 | 01 | 11 | 10 |
|:-:|:-:|:-:|:-:|:-:|
| **00** | m0 | m1 | m3 | m2 |
| **01** | m4 | m5 | m7 | m6 |
| **11** | m12 | m13 | m15 | m14 |
| **10** | m8 | m9 | m11 | m10 |

## 2. Reglas de agrupación

1. Agrupa solo unos adyacentes (o ceros, si buscas producto de sumas).
2. El tamaño del grupo es potencia de 2: 1, 2, 4, 8, 16.
3. Haz los grupos **lo más grandes posible** y usa el menor número de grupos que cubra todos los unos.
4. Los grupos pueden envolver los bordes y pueden solaparse. Nunca en diagonal.

> [!NOTE]
> En un grupo, las variables que **cambian** desaparecen; las que se mantienen constantes se quedan (directas si valen 1, negadas si valen 0). Un grupo de 2 elimina 1 variable, uno de 4 elimina 2, uno de 8 elimina 3.

> **Ejemplo resuelto**
>
> - **F(A,B,C) = Σm(1,3,5,7):** los cuatro unos forman la columna donde C = 1. A y B cambian, C no. Resultado: **F = C**.
> - **F(A,B,C) = Σm(0,2,4,6):** columnas donde C = 0. Resultado: **F = C′**.

## 3. Condiciones indiferentes (X)

> [!NOTE]
> **Definición.** Una **condición indiferente** es una combinación de entrada que nunca ocurre o cuya salida no importa. Se marca con X y se puede tomar como 1 si ayuda a hacer un grupo más grande, o ignorar como 0 si no ayuda. No obliga a cubrirla.

> **Ejemplo resuelto:** un display de 7 segmentos con entrada BCD. Las combinaciones 1010 a 1111 nunca llegan. Son 6 X que permiten simplificar mucho cada segmento.

## 4. Universalidad de NAND y NOR

> [!NOTE]
> **Definición.** Una compuerta es **universal** si con ella sola puede construirse cualquier función lógica. NAND y NOR lo son. Esto permite fabricar un circuito completo con **un solo tipo de integrado** (el 74LS00 o el 74LS02), lo que ahorra costo y espacio.

| Función | Solo con NAND | Solo con NOR |
|:--|:--|:--|
| **NOT A** | (A·A)′ | (A+A)′ |
| **A AND B** | ((A·B)′·(A·B)′)′ | ((A+A)′ + (B+B)′)′ |
| **A OR B** | ((A·A)′·(B·B)′)′ | ((A+B)′+(A+B)′)′ |

> **Ejemplo resuelto: convertir un esquema AND-OR a NAND-NAND.** Toma la suma de productos F = AB + CD. Reemplaza cada AND por NAND y la OR final por NAND. Por De Morgan: ((AB)′ · (CD)′)′ = AB + CD.
>
> **Procedimiento:** (1) simplifica a suma de productos, (2) niega dos veces F, (3) aplica De Morgan una vez y obtén un NAND de NANDs.
> Para NOR se parte del producto de sumas, o se niega dos veces la forma mínima y se aplica De Morgan al otro lado.

## 5. Ejemplo guiado de cuatro variables

> **Ejemplo resuelto: F(A,B,C,D) = Σm(0,1,2,5,8,9,10) + d(13,15)**

| AB \ CD | 00 | 01 | 11 | 10 |
|:-:|:-:|:-:|:-:|:-:|
| **00** | **1** (m0) | **1** (m1) | 0 (m3) | **1** (m2) |
| **01** | 0 (m4) | **1** (m5) | 0 (m7) | 0 (m6) |
| **11** | 0 (m12) | X (m13) | X (m15) | 0 (m14) |
| **10** | **1** (m8) | **1** (m9) | 0 (m11) | **1** (m10) |

- Las cuatro esquinas y la fila de arriba con la de abajo (los ceros de B) forman el grupo de 4 celdas **B′D′** (m0, m2, m8, m10).
- La columna D = 1 con C = 0 forma otro grupo de 4 celdas, **C′D** (m1, m5, m9 y la X de m13, que aquí se toma como 1).
- La X de m15 se ignora porque no ayuda.

Resultado: **F = B′D′ + C′D**. Sin las X el segundo grupo tendría solo 2 celdas y la expresión quedaría más larga.

> [!IMPORTANT]
> **Proyecto integrador de saberes.** En esta semana se presenta el producto, se entregan la guía y la rúbrica, se conforman los equipos y se asignan roles técnicos rotativos. Todo lo que aprendes en las semanas 1 a 4 (tabla de verdad, minimización y bloques aritméticos) es la base para diseñar y justificar tu solución.

## 6. Profundizando la clase

### 6.1 Procedimiento de minimización en seis pasos

1. Dibuja el mapa con los encabezados en código Gray.
2. Coloca 1 en los minitérminos de la función, X en las indiferentes y deja 0 en el resto.
3. Marca primero los unos que solo pueden agruparse de una manera (aislados o con un solo grupo posible).
4. Agrupa con el mayor tamaño posible, potencias de 2, buscando envolver bordes.
5. Cubre todos los unos con el menor número de grupos. Las X ayudan, pero no se cubren por obligación.
6. Escribe cada grupo como un producto con las variables que no cambian y suma los productos.

> [!NOTE]
> Un grupo no ampliable es un **implicante primo**. Si tiene un uno que ningún otro grupo cubre, es **esencial** y debe estar en la respuesta.

### 6.2 Ejemplo de tres variables con envoltura

> **Ejemplo resuelto: F(A,B,C) = Σm(0,1,2,6,7)**

| A \ BC | 00 | 01 | 11 | 10 |
|:-:|:-:|:-:|:-:|:-:|
| **0** | **1** (m0) | **1** (m1) | 0 (m3) | **1** (m2) |
| **1** | 0 (m4) | 0 (m5) | **1** (m7) | **1** (m6) |

Grupos: **A′B′** (m0 y m1), **AB** (m6 y m7) y un tercero para cubrir m2, que puede ser **A′C′** (m0 y m2) o **BC′** (m2 y m6, envolviendo desde la fila de arriba). Los dos son válidos:

- **F = A′B′ + AB + BC′**
- **F = A′B′ + AB + A′C′**

Las dos expresiones son equivalentes y tienen el mismo costo; el mapa muestra que la mínima no siempre es única.

### 6.3 Mínima en producto de sumas agrupando ceros

Si se agrupan los ceros del mismo mapa (m3, m4, m5) se obtiene F′ y luego se niega con De Morgan.

> **Ejemplo resuelto**
>
> Ceros: m4 y m5 forman AB′; m3 queda solo y es A′BC. Entonces F′ = AB′ + A′BC.
> Se niega: F = (AB′)′ · (A′BC)′ = **(A′ + B)(A + B′ + C′)**.
> Es otra forma de la misma función, útil cuando hay pocos ceros.

### 6.4 Mapa de la alarma de tres sensores

| A \ BC | 00 | 01 | 11 | 10 |
|:-:|:-:|:-:|:-:|:-:|
| **0** | 0 | 0 | **1** (m3) | 0 |
| **1** | 0 | **1** (m5) | **1** (m7) | **1** (m6) |

Tres grupos de dos celdas: **AB** (m6, m7), **AC** (m5, m7) y **BC** (m3, m7). Se llega a F = AB + AC + BC en segundos, mientras que con álgebra hubo que reconocer la ley de consenso. El mapa hace visible lo que el álgebra esconde.

### 6.5 Conversión completa a NAND y a NOR

> **Ejemplo resuelto**
>
> - **A NAND:** F = AB + C′D → F = ((AB)′ · (C′D)′)′. Se necesitan: NAND(A,B), NAND(C′,D), una NAND que haga de inversor para C (entradas unidas) y una NAND final. Son 4 NAND, que caben en **un solo 74LS00** (trae 4 compuertas).
> - **A NOR:** F = (A + B)(C + D) → F = ((A + B)′ + (C + D)′)′. Tres NOR en **un solo 74LS02**.

> [!WARNING]
> Antes de convertir, comprueba si necesitas inversores. Cada variable negada que no esté disponible en la entrada cuesta una compuerta más.

### 6.6 Ejercicios de práctica con respuesta

<details><summary><b>a)</b> Minimiza Σm(1,3,5,7) con 3 variables.</summary>

**F = C.**
</details>

<details><summary><b>b)</b> Minimiza Σm(0,1,4,5) con 3 variables.</summary>

Se mantienen B = 0: **F = B′.**
</details>

<details><summary><b>c)</b> ¿Cuántas variables elimina un grupo de 4 en un mapa de 4 variables?</summary>

2 variables, queda un producto de 2 literales.
</details>

<details><summary><b>d)</b> Convierte F = A + B a solo NAND.</summary>

F = (A′ · B′)′: dos NAND como inversores y una NAND final, 3 en total.
</details>

## 7. Preguntas clave para entender el tema

<details><summary><b>¿Qué problema del álgebra resuelve el mapa de Karnaugh?</b></summary>

Muestra cuándo se llegó a la mínima, porque la simplificación se convierte en agrupar celdas vecinas visibles.
</details>

<details><summary><b>¿Por qué el orden de los encabezados es 00, 01, 11, 10?</b></summary>

Para que dos celdas vecinas (incluso las de los bordes) difieran en un solo bit. Solo así se elimina una variable al juntarlas.
</details>

<details><summary><b>¿Por qué los grupos deben ser de 1, 2, 4, 8 o 16 celdas?</b></summary>

Cada vez que se duplica el grupo se elimina una variable. Un grupo de tamaño no potencia de 2 no corresponde a ningún producto simple.
</details>

<details><summary><b>¿Cuándo tomo una X como 1 y cuándo la ignoro?</b></summary>

La tomo como 1 si me deja hacer un grupo más grande. Si no ayuda, la ignoro: no hace falta cubrirla.
</details>

<details><summary><b>¿Por qué se puede construir todo con solo NAND o solo NOR?</b></summary>

Con ellas se obtienen NOT, AND y OR, y con esas tres se puede expresar cualquier función.
</details>

<details><summary><b>¿Qué se gana implementando con una sola familia?</b></summary>

Un solo tipo de integrado en el circuito: menos costo, menos inventario y un diseño más ordenado.
</details>

---

<p align="center"><a href="README.md"><img src="assets/tab-README-off.svg" alt="Inicio" height="40"></a> <a href="semana-1.md"><img src="assets/tab-semana-1-off.svg" alt="Semana 1" height="40"></a> <a href="semana-2.md"><img src="assets/tab-semana-2-off.svg" alt="Semana 2" height="40"></a> <a href="semana-3.md"><img src="assets/tab-semana-3-on.svg" alt="Semana 3" height="40"></a> <a href="semana-4.md"><img src="assets/tab-semana-4-off.svg" alt="Semana 4" height="40"></a> <a href="glosario.md"><img src="assets/tab-glosario-off.svg" alt="Glosario" height="40"></a></p>

<p align="center"><sub>Diseño de Circuitos · Erick Andrade Montero · Universidad Nacional de Loja</sub></p>
