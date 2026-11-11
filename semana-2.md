[Inicio](README.md) · [Semana 1](semana-1.md) · **Semana 2** · [Semana 3](semana-3.md) · [Semana 4](semana-4.md) · [Glosario](glosario.md)

---

# Semana 2 · Álgebra de Boole y formas canónicas

Un circuito y una expresión matemática son lo mismo escrito de dos maneras. El álgebra de Boole permite pasar de un enunciado en lenguaje natural a una especificación sin ambigüedad, y luego reducirla.

## 1. Postulados y leyes

Notación: `+` es OR, `·` es AND (puede omitirse: `AB`) y `A′` es NOT.

| Ley | Forma con `+` | Forma con `·` |
|:--|:--|:--|
| Identidad | A + 0 = A | A · 1 = A |
| Elemento nulo | A + 1 = 1 | A · 0 = 0 |
| Idempotencia | A + A = A | A · A = A |
| Complemento | A + A′ = 1 | A · A′ = 0 |
| Involución | (A′)′ = A | (A′)′ = A |
| Conmutativa | A + B = B + A | AB = BA |
| Asociativa | (A+B)+C = A+(B+C) | (AB)C = A(BC) |
| Distributiva | A + BC = (A+B)(A+C) | A(B+C) = AB + AC |
| Absorción | A + AB = A | A(A+B) = A |
| Simplificación | A + A′B = A + B | A(A′+B) = AB |
| Consenso | AB + A′C + BC = AB + A′C | (A+B)(A′+C)(B+C) = (A+B)(A′+C) |

> [!NOTE]
> Cada ley de la columna izquierda tiene su **dual** en la derecha: se obtiene cambiando `+` por `·` y 0 por 1. Si demuestras una, la otra queda demostrada.

## 2. Teoremas de De Morgan

> [!NOTE]
> **Definición.**
>
> ```text
> (A · B)′ = A′ + B′
> (A + B)′ = A′ · B′
> ```
>
> Negar una AND equivale a una OR de entradas negadas. Negar una OR equivale a una AND de entradas negadas. Se generalizan a *n* variables.

**Comprobación con tabla de verdad:** las columnas `(A·B)′` y `A′+B′` son idénticas, y también `(A+B)′` y `A′·B′`.

| A | B | (A·B)′ | A′+B′ | (A+B)′ | A′·B′ |
|:-:|:-:|:-:|:-:|:-:|:-:|
| 0 | 0 | 1 | 1 | 1 | 1 |
| 0 | 1 | 1 | 1 | 0 | 0 |
| 1 | 0 | 1 | 1 | 0 | 0 |
| 1 | 1 | 0 | 0 | 0 | 0 |

> **Ejemplo resuelto: aplicar De Morgan.** (A + B′C)′ = A′ · (B′C)′ = A′ · (B + C′) = A′B + A′C′.

## 3. Simplificación algebraica paso a paso

> **Ejemplo resuelto: F = A′B + AB + AB′**
>
> 1. Agrupar A′B + AB = B(A′ + A) = B · 1 = B
> 2. F = B + AB′
> 3. Por la ley A + A′B = A + B (aquí con B + AB′): **F = A + B**
>
> Una OR sola reemplaza 3 ANDs, 1 NOT y 1 OR de 3 entradas.

> [!WARNING]
> La gran dificultad del método algebraico: **no avisa cuándo llegaste a la expresión mínima**. Dos personas pueden detenerse en puntos distintos. Por eso en la semana 3 aparecen los mapas de Karnaugh.

## 4. Minitérminos y maxitérminos

> [!NOTE]
> **Minitérmino (mᵢ):** producto de todas las variables, cada una directa o negada, que vale 1 en exactamente una fila de la tabla. Si la variable vale 1 aparece directa; si vale 0, negada.
>
> **Maxitérmino (Mᵢ):** suma de todas las variables que vale 0 en exactamente una fila. Si la variable vale 0 aparece directa; si vale 1, negada.

| i | A | B | C | Minitérmino | Maxitérmino |
|:-:|:-:|:-:|:-:|:--|:--|
| 0 | 0 | 0 | 0 | m0 = A′B′C′ | M0 = A + B + C |
| 1 | 0 | 0 | 1 | m1 = A′B′C | M1 = A + B + C′ |
| 2 | 0 | 1 | 0 | m2 = A′BC′ | M2 = A + B′ + C |
| 3 | 0 | 1 | 1 | m3 = A′BC | M3 = A + B′ + C′ |
| 4 | 1 | 0 | 0 | m4 = AB′C′ | M4 = A′ + B + C |
| 5 | 1 | 0 | 1 | m5 = AB′C | M5 = A′ + B + C′ |
| 6 | 1 | 1 | 0 | m6 = ABC′ | M6 = A′ + B′ + C |
| 7 | 1 | 1 | 1 | m7 = ABC | M7 = A′ + B′ + C′ |

Relación útil: `Mᵢ = (mᵢ)′`.

## 5. Suma de productos y producto de sumas

> [!NOTE]
> - **SOP (suma de productos canónica):** OR de los minitérminos de las filas donde F = 1. Notación compacta: `F = Σm(3,5,6,7)`.
> - **POS (producto de sumas canónica):** AND de los maxitérminos de las filas donde F = 0. Notación: `F = ΠM(0,1,2,4)`.

> [!TIP]
> **Criterio para elegir:** usa la forma que tenga menos términos. Si F tiene pocos unos, conviene la suma de productos; si tiene pocos ceros, conviene el producto de sumas. Las dos describen la misma función.

## 6. Ejemplo guiado: alarma de tres sensores

> **Ejemplo resuelto**
>
> **Enunciado:** una alarma suena cuando al menos dos de los tres sensores (A, B, C) detectan movimiento.
>
> - **Tabla:** F = 1 en las filas 011, 101, 110 y 111.
> - **Canónica:** F = Σm(3,5,6,7) = A′BC + AB′C + ABC′ + ABC
> - **Simplificada:** F = AB + AC + BC
>
> Con la forma canónica se necesitan 4 ANDs de 3 entradas y 1 OR de 4; con la simplificada, 3 ANDs de 2 entradas y 1 OR de 3.

**Ejemplo de comparador de expresiones.** F = A·B′ + A′·B + A·B comparada con G = A + B:

| A | B | i | F | G |
|:-:|:-:|:-:|:-:|:-:|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | **1** | **1** |
| 1 | 0 | 2 | **1** | **1** |
| 1 | 1 | 3 | **1** | **1** |

F = Σm(1,2,3) = ΠM(0). F y G tienen la misma tabla de verdad, por lo tanto son **equivalentes**.

## 7. Profundizando la clase

### 7.1 Método para pasar de un enunciado a un circuito

1. Identifica las entradas y la salida, y asígnales una letra a cada una.
2. Decide qué significa 1 y 0 para cada señal (por ejemplo, 1 = sensor activado).
3. Escribe la tabla de verdad fila por fila, leyendo el enunciado.
4. Extrae los minitérminos (o maxitérminos) y escribe la forma canónica.
5. Simplifica, dibuja el circuito y cuenta las compuertas.

> **Ejemplo resuelto: sistema de riego.** La válvula V se abre si la humedad es baja (H) y no está lloviendo (L), o si el modo manual (M) está activo. Con A = H, B = L, C = M: V = 1 cuando C = 1 (filas 1, 3, 5, 7) o cuando A = 1 y B = 0 (filas 4 y 5).
>
> - **Canónica SOP:** V = Σm(1,3,4,5,7) = A′B′C + A′BC + AB′C′ + AB′C + ABC
> - **Canónica POS:** V = ΠM(0,2,6) = (A+B+C)(A+B′+C)(A′+B′+C)
> - **Simplificada:** V = AB′ + C. Pasó de 5 productos de 3 literales a un producto de 2 y una variable sola: una AND, una NOT y una OR.

| i | A | B | C | V |
|:-:|:-:|:-:|:-:|:-:|
| 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 1 | **1** |
| 2 | 0 | 1 | 0 | 0 |
| 3 | 0 | 1 | 1 | **1** |
| 4 | 1 | 0 | 0 | **1** |
| 5 | 1 | 0 | 1 | **1** |
| 6 | 1 | 1 | 0 | 0 |
| 7 | 1 | 1 | 1 | **1** |

### 7.2 Pasar de SOP a POS y viceversa

Las filas que no están en la lista de minitérminos son las de los maxitérminos. Para 3 variables hay 8 filas, entonces si F = Σm(1,3,4,5,7), el resto (0, 2, 6) son los maxitérminos: F = ΠM(0,2,6). Los números complementan al conjunto {0,…,7}.

### 7.3 Demostraciones que conviene saber

> **Ejemplo resuelto**
>
> - **Absorción:** A + AB = A(1 + B) = A · 1 = A.
> - **Simplificación:** A + A′B = (A + A′)(A + B) = 1 · (A + B) = A + B (distributiva).
> - **Distributiva de la suma:** (A + B)(A + C) = AA + AC + AB + BC = A + AC + AB + BC = A + BC.
> - **Consenso:** AB + A′C + BC. Se multiplica BC por (A + A′): BC = ABC + A′BC. Cada uno de estos queda absorbido por AB y por A′C respectivamente, y el resultado es AB + A′C.

### 7.4 Costo de la forma canónica frente a la mínima

| Alarma de tres sensores | Canónica | Simplificada |
|:--|:--|:--|
| Expresión | A′BC + AB′C + ABC′ + ABC | AB + AC + BC |
| NOT | 3 | 0 |
| AND | 4 de 3 entradas | 3 de 2 entradas |
| OR | 1 de 4 entradas | 1 de 3 entradas |
| **Compuertas totales** | **8** | **4** |

La mitad de compuertas hacen la misma tarea, con menos cableado, menos consumo y menos retardo.

### 7.5 Ejercicios de práctica con respuesta

<details><summary><b>a)</b> Simplifica (A + B)(A + B′).</summary>

= A + BB′ = A + 0 = **A**.
</details>

<details><summary><b>b)</b> Simplifica A′B′ + A′B.</summary>

= A′(B′ + B) = **A′**.
</details>

<details><summary><b>c)</b> Simplifica AB + AB′C.</summary>

= A(B + B′C) = A(B + C) = **AB + AC**.
</details>

<details><summary><b>d)</b> Niega con De Morgan: F = A′B + CD′.</summary>

F′ = **(A + B′)(C′ + D)**.
</details>

## 8. Preguntas clave para entender el tema

<details><summary><b>¿Por qué se dice que un circuito y una expresión son lo mismo escrito de dos maneras?</b></summary>

Cada operador de la expresión corresponde a una compuerta, y cada variable a una entrada. Se puede pasar de una a otro sin perder información.
</details>

<details><summary><b>¿Qué es un minitérmino y por qué vale 1 en una sola fila?</b></summary>

Es un producto de todas las variables, directas o negadas, ajustado para que solo una combinación lo vuelva verdadero. Cualquier otra combinación pone al menos un literal en 0.
</details>

<details><summary><b>¿Cuándo uso suma de productos y cuándo producto de sumas?</b></summary>

Elijo la que tenga menos términos: SOP si hay pocos unos en la tabla, POS si hay pocos ceros.
</details>

<details><summary><b>¿Qué cambia al aplicar De Morgan?</b></summary>

Se cambia el operador (AND por OR y viceversa) y se niegan las variables. Es la herramienta para convertir entre familias de compuertas.
</details>

<details><summary><b>¿Cómo comprobar que dos expresiones son equivalentes?</b></summary>

Si sus tablas de verdad son idénticas en todas las filas, son la misma función.
</details>

<details><summary><b>¿Cómo sé que llegué a la expresión mínima con álgebra?</b></summary>

No hay forma directa: esa es la limitación del método algebraico y el motivo para usar mapas de Karnaugh.
</details>

---

[← Semana 1](semana-1.md) · [Semana 3 →](semana-3.md)

<div align="center"><sub>Diseño de Circuitos · Erick Andrade Montero · Universidad Nacional de Loja</sub></div>
