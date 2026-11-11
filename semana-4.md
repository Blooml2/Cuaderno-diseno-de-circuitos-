[Inicio](README.md) · [Semana 1](semana-1.md) · [Semana 2](semana-2.md) · [Semana 3](semana-3.md) · **Semana 4** · [Glosario](glosario.md)

---

# Semana 4 · Bloques combinacionales aritméticos

Un circuito sin memoria llega a sumar. El problema central de todo sumador es el **acarreo**: cada columna necesita saber qué le llega de la columna anterior.

## 1. Semisumador (half adder)

> [!NOTE]
> **Definición.** Suma dos bits A y B y entrega la **suma S** y el **acarreo C**. No admite acarreo de entrada.
>
> ```text
> S = A ⊕ B        C = A · B
> ```
>
> Se construye con una XOR y una AND.

| A | B | S | C |
|:-:|:-:|:-:|:-:|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | **1** |

## 2. Sumador completo (full adder)

> [!NOTE]
> **Definición.** Suma tres bits: A, B y el acarreo de entrada Cᵢₙ. Entrega S y el acarreo de salida Cₒᵤₜ.
>
> ```text
> S    = A ⊕ B ⊕ Cin
> Cout = AB + Cin(A ⊕ B)
> ```

| A | B | Cin | S | Cout |
|:-:|:-:|:-:|:-:|:-:|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 |
| 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 1 | 0 | **1** |
| 1 | 0 | 0 | 1 | 0 |
| 1 | 0 | 1 | 0 | **1** |
| 1 | 1 | 0 | 0 | **1** |
| 1 | 1 | 1 | 1 | **1** |

> **Ejemplo resuelto: diseño desde la tabla de verdad.** S vale 1 en las filas con cantidad impar de unos (m1, m2, m4, m7) y Cout vale 1 en las filas con dos o más unos (m3, m5, m6, m7). Con Karnaugh, Cout = AB + A·Cin + B·Cin: es la misma función de mayoría de la alarma de la semana 2.
> **Esquema:** dos semisumadores en cadena y una OR para los acarreos.

## 3. Sumador paralelo de 4 bits

Se conectan 4 sumadores completos en cadena: el Cout de cada etapa entra como Cin de la siguiente. Esta disposición se llama **acarreo en cascada** (*ripple carry*).

> [!NOTE]
> **Retardo de propagación.** El acarreo debe viajar por todas las etapas. Si cada compuerta tarda tₚ y el camino del acarreo de una etapa tiene 2 compuertas, el peor caso de 4 bits es aproximadamente **8·tₚ** (2·n·tₚ para *n* bits). Con tₚ = 10 ns serían 80 ns. Más bits, más lento. Por eso existen los sumadores de acarreo anticipado.

**Ejemplo: A = 1001 (9) más B = 0110 (6), con el acarreo de cada etapa.**

| | bit 3 | bit 2 | bit 1 | bit 0 |
|:--|:-:|:-:|:-:|:-:|
| Acarreos de entrada | 0 | 0 | 0 | 0 |
| A | 1 | 0 | 0 | 1 |
| B | 0 | 1 | 1 | 0 |
| **Suma S** | **1** | **1** | **1** | **1** |

Resultado: `1111` = 15, acarreo final 0. Comparador: A > B.

## 4. Resta por complemento a dos

> [!NOTE]
> **Definición.** Restar es sumar el negativo: `A − B = A + B′ + 1`, donde B′ es B con todos los bits invertidos. En hardware: se pasa B por XOR con una señal "restar" y esa misma señal se conecta al Cin inicial.

> **Ejemplo resuelto: 9 − 6 en 4 bits.** A = 1001, B = 0110 → B′ = 1001. Suma: 1001 + 1001 + 1 = 1 0011. El bit de acarreo final (1) se descarta. Resultado: **0011 = 3**.
>
> **Regla del acarreo final:** si hay acarreo, el resultado es positivo (A ≥ B). Si no hay, es negativo y está en complemento a dos.

## 5. Comparador de magnitud

> [!NOTE]
> **Definición.** Compara dos números A y B y activa una de tres salidas: A > B, A = B o A < B. Para 1 bit:
>
> ```text
> (A = B) = (A ⊕ B)′     (A > B) = A·B′     (A < B) = A′·B
> ```
>
> Para varios bits se compara desde el bit más significativo; solo si son iguales se pasa al siguiente. El integrado comercial es el **74LS85**.

## 6. Relación con la unidad aritmético-lógica (ALU)

Una ALU combina estos mismos bloques: sumador/restador, comparador y compuertas lógicas (AND, OR, XOR) en un solo circuito. Un multiplexor elige cuál resultado sale según un código de operación. Lo que construyes esta semana es el corazón de la ALU de un procesador.

> **Ejemplo resuelto: análisis de un sumador paralelo.** Sumar 0111 + 0101.
> Etapa 0: 1+1 = 0, acarreo 1. Etapa 1: 1+0+1 = 0, acarreo 1. Etapa 2: 1+1+1 = 1, acarreo 1. Etapa 3: 0+0+1 = 1, acarreo 0.
> Resultado: `1100` = 12. Correcto: 7 + 5 = 12.

## 7. Profundizando la clase

### 7.1 Diseño del sumador completo con mapas

La salida **S** vale 1 en las filas 1, 2, 4 y 7. Esos unos están en forma de tablero de ajedrez: ninguno es vecino de otro, por eso no se agrupan y la mejor forma es la XOR triple `S = A ⊕ B ⊕ Cin`. La salida **Cout** sí se agrupa.

| A \ B·Cin | 00 | 01 | 11 | 10 |
|:-:|:-:|:-:|:-:|:-:|
| **0** | 0 | 0 | **1** (m3) | 0 |
| **1** | 0 | **1** (m5) | **1** (m7) | **1** (m6) |

Grupos de dos celdas: **B·Cin** (m3 y m7), **A·Cin** (m5 y m7) y **A·B** (m6 y m7). Entonces `Cout = AB + A·Cin + B·Cin`.

### 7.2 El sumador completo construido con dos semisumadores

> **Ejemplo resuelto**
>
> - El primer semisumador suma A y B: `S₁ = A ⊕ B` y `C₁ = A · B`.
> - El segundo suma S₁ y Cin: `S = S₁ ⊕ Cin` y `C₂ = S₁ · Cin`.
> - El acarreo final es `Cout = C₁ + C₂ = AB + Cin(A ⊕ B)`, que es la expresión de la teoría.
>
> **Costo:** 2 XOR (74LS86), 2 AND (74LS08) y 1 OR (74LS32): son 3 integrados para un solo bit. Por eso para 4 bits se usa un integrado comercial, el **74LS83**, que trae los cuatro sumadores completos.

### 7.3 Desbordamiento (overflow) con números con signo

Cuando A y B están en complemento a dos con 4 bits, el rango es de −8 a +7. Si el resultado cae fuera, el resultado es incorrecto aunque el circuito funcione perfecto.

> **Ejemplo resuelto: 0111 + 0011 (+7 + +3).** Resultado: 1010, que en complemento a dos vale −6. Hay desbordamiento porque la suma real es +10, que no cabe.
>
> **Regla de detección:** hay desbordamiento cuando el acarreo que entra al último bit y el acarreo que sale del último bit son diferentes. Aquí entra 1 y sale 0.

### 7.4 Más ejemplos de resta

> **Ejemplo resuelto**
>
> - **3 − 5:** 0011 + (5 invertido: 1010) + 1 = 0011 + 1011 = 1110. No hay acarreo final, entonces el resultado es negativo: 1110 en complemento a dos es **−2**.
> - **6 − 4:** 0110 + 1011 + 1 = 1 0010. Hay acarreo final, resultado positivo: **0010 = 2**.

### 7.5 Comparador de 2 bits

> [!NOTE]
> Con A = A₁A₀ y B = B₁B₀:
>
> ```text
> A = B:  (A1 ⊙ B1)(A0 ⊙ B0)
> A > B:  A1·B1′ + (A1 ⊙ B1)·A0·B0′
> A < B:  A1′·B1 + (A1 ⊙ B1)·A0′·B0
> ```
>
> El símbolo ⊙ es la XNOR (igualdad). Primero se mira el bit más significativo; solo si empatan se decide con el siguiente.

> **Ejemplo resuelto:** ¿A = 10 y B = 01? A₁ ≠ B₁, y A₁·B₁′ = 1·1 = 1, entonces **A > B** (2 > 1).

### 7.6 Acarreo anticipado: cómo ir más rápido

El sumador en cascada espera a que el acarreo cruce todas las etapas. Una mejora define para cada bit **Gᵢ = AᵢBᵢ** (genera acarreo) y **Pᵢ = Aᵢ ⊕ Bᵢ** (propaga el acarreo). Entonces `Cᵢ₊₁ = Gᵢ + Pᵢ·Cᵢ`. Expandiendo esa fórmula, todos los acarreos se calculan en paralelo en dos niveles de compuertas, sin esperar a la etapa anterior. El costo es más compuertas a cambio de menor retardo.

### 7.7 La ALU como un conjunto de bloques

| Selector S₁S₀ | Operación | Bloque que la produce |
|:-:|:-:|:--|
| 00 | A + B | Sumador paralelo |
| 01 | A − B | Sumador con B invertido y Cin = 1 |
| 10 | A AND B | Compuertas AND |
| 11 | A OR B | Compuertas OR |

Un multiplexor de 4 entradas elige cuál de los cuatro resultados llega a la salida según S₁S₀. Es el esquema simplificado de la ALU de un procesador.

### 7.8 Ejercicios de práctica con respuesta

<details><summary><b>a)</b> Suma 1011 + 0110 en 4 bits.</summary>

`1 0001`: el resultado de 4 bits es 0001 y el acarreo final 1 indica que el valor real (17) no cabe en 4 bits sin signo.
</details>

<details><summary><b>b)</b> Resta 0110 − 0100.</summary>

0110 + 1011 + 1 = 1 0010 → **0010 = 2**.
</details>

<details><summary><b>c)</b> ¿Cuánto tarda un sumador de 8 bits en cascada si tₚ = 10 ns?</summary>

2·8·10 = **160 ns** aproximadamente.
</details>

<details><summary><b>d)</b> Escribe S y Cout para A = 1, B = 1, Cin = 1.</summary>

**S = 1 y Cout = 1** (1+1+1 = 11₂).
</details>

## 8. Preguntas clave para entender el tema

<details><summary><b>¿Por qué el acarreo es el problema central de un sumador?</b></summary>

Porque cada columna depende de lo que sale de la anterior. Calcular el acarreo bien y rápido define el diseño y la velocidad.
</details>

<details><summary><b>¿Qué diferencia hay entre un semisumador y un sumador completo?</b></summary>

El semisumador no recibe acarreo de entrada, por eso solo sirve para el primer bit. El completo lo recibe y se puede encadenar.
</details>

<details><summary><b>¿Cómo se convierte un sumador en un restador?</b></summary>

Se invierte B con compuertas XOR y se pone el acarreo de entrada en 1: A − B = A + B′ + 1.
</details>

<details><summary><b>¿Cómo sé si una resta dio un resultado negativo?</b></summary>

Si no hay acarreo final, A es menor que B y el resultado está en complemento a dos.
</details>

<details><summary><b>¿Por qué un sumador en cascada se vuelve más lento con más bits?</b></summary>

El acarreo debe atravesar todas las etapas, y el retardo crece de forma proporcional al número de bits.
</details>

<details><summary><b>¿Qué papel cumplen estos bloques dentro de una ALU?</b></summary>

Son sus piezas: el sumador/restador y el comparador hacen la aritmética, y las compuertas hacen las operaciones lógicas.
</details>

---

[← Semana 3](semana-3.md) · [Glosario →](glosario.md)

<div align="center"><sub>Diseño de Circuitos · Erick Andrade Montero · Universidad Nacional de Loja</sub></div>
