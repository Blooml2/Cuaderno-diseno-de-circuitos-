<div align="center">

![Universidad Nacional de Loja](https://img.shields.io/badge/UNIVERSIDAD-NACIONAL%20DE%20LOJA-6E1E36?style=for-the-badge)
![Carrera](https://img.shields.io/badge/CARRERA-COMPUTACI%C3%93N-066670?style=for-the-badge)

# Diseño de Circuitos

### Cuaderno digital · Circuitos combinacionales básicos

![Lógica combinacional](https://img.shields.io/badge/L%C3%B3gica-combinacional-0B8F9A?style=flat-square)
![Álgebra de Boole](https://img.shields.io/badge/%C3%81lgebra-de%20Boole-6E1E36?style=flat-square)
![Mapas de Karnaugh](https://img.shields.io/badge/Mapas%20de-Karnaugh-0B8F9A?style=flat-square)
![Sumadores](https://img.shields.io/badge/Bloques-aritm%C3%A9ticos-6E1E36?style=flat-square)

| | |
|:--|:--|
| **Estudiante** | Erick Andrade Montero |
| **Docente** | Ing. Luis Sinche Cueva |
| **Materia** | Diseño de Circuitos |
| **Ciclo** | Ciclo 2 · Computación |
| **Institución** | Universidad Nacional de Loja |

</div>

---

## Contenido

1. [Semana 1 · Análisis de lógica combinacional](#semana-1--análisis-de-lógica-combinacional)
   Sistemas numéricos, aritmética binaria, complemento a dos, BCD, Gray, ASCII, compuertas, tablas de verdad y hoja de datos del 74LS.
2. [Semana 2 · Álgebra de Boole y formas canónicas](#semana-2--álgebra-de-boole-y-formas-canónicas)
   Postulados y leyes, De Morgan, simplificación algebraica, minitérminos, maxitérminos, SOP y POS.
3. [Semana 3 · Mapas de Karnaugh y universalidad NAND/NOR](#semana-3--mapas-de-karnaugh-y-universalidad-nandnor)
   Mapas de 3 y 4 variables, condiciones indiferentes, implementación con una sola familia de compuertas.
4. [Semana 4 · Bloques combinacionales aritméticos](#semana-4--bloques-combinacionales-aritméticos)
   Semisumador, sumador completo, sumador paralelo, resta por complemento a dos, comparador de magnitud y ALU.
5. [Glosario](#glosario)

> **Notación usada.** `+` es OR, `·` es AND (a veces se omite: `AB`), `A′` es NOT A (también se escribe Ā), `⊕` es XOR y `⊙` es XNOR.

---

# Semana 1 · Análisis de lógica combinacional

Un circuito **combinacional** es aquel cuya salida depende solo de las entradas del momento. No tiene memoria. Esta semana se aprende el idioma con el que se escriben: sistemas numéricos, códigos y compuertas.

## 1. ¿Por qué todo es binario?

Un computador representa toda la información con **dos estados eléctricos**: un voltaje alto (1) y un voltaje bajo (0). Dos estados son fáciles de distinguir aunque haya ruido, por eso se prefieren a diez niveles de voltaje.

> [!NOTE]
> **Definición.** Un **bit** es un dígito binario (0 o 1). Un grupo de 4 bits es un **nibble** y uno de 8 bits es un **byte**.

## 2. Sistemas numéricos

| Sistema | Base | Dígitos | Detalle |
|:--|:-:|:--|:--|
| **Binario** | 2 | 0, 1 | Cada posición vale una potencia de 2: …16, 8, 4, 2, 1 |
| **Octal** | 8 | 0 a 7 | Cada dígito agrupa 3 bits |
| **Hexadecimal** | 16 | 0 a 9 y A a F | Cada dígito agrupa 4 bits |

> **Ejemplo resuelto**
>
> - **Binario a decimal:** `1011₂` = 1·8 + 0·4 + 1·2 + 1·1 = **11**.
> - **Decimal a binario** (divisiones sucesivas entre 2): 25 → residuos 1, 0, 0, 1, 1 leídos de abajo hacia arriba → `11001₂`.
> - **Binario a hexadecimal:** `1101 0110` → D y 6 → `D6₁₆`.
> - **Binario a octal:** `011 101 110` → 3, 5, 6 → `356₈`.

**Ejemplo con el decimal 25 en todos los sistemas y códigos:**

| Decimal | Binario | Octal | Hexadecimal | BCD | Gray | Compl. a dos (8 bits) |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| 25 | `11001` | `31` | `19` | `0010 0101` | `10101` | `00011001` |

## 3. Aritmética binaria y complemento a dos

Suma: `0+0=0`, `0+1=1`, `1+0=1`, `1+1=0` y llevo 1.

> **Ejemplo resuelto:** `0110 + 0111` (6 + 7)
>
> ```text
>   1 1 0   ← acarreos
>   0 1 1 0
> + 0 1 1 1
> ---------
>   1 1 0 1   = 13
> ```

> [!NOTE]
> **Definición.** El **complemento a dos** permite representar negativos. Para negar un número se invierten todos sus bits (complemento a uno) y se suma 1. Con *n* bits el rango es de −2ⁿ⁻¹ a 2ⁿ⁻¹ − 1.

> **Ejemplo resuelto:** −5 en 8 bits. 5 = `00000101` → invertir: `11111010` → sumar 1: `11111011`.
> Comprobación: 5 + (−5) = `00000101 + 11111011 = 1 00000000`; el bit 9 se descarta y queda 0.

## 4. Códigos binarios

> [!NOTE]
> - **BCD:** cada dígito decimal se codifica con 4 bits. 59 = `0101 1001`. Los patrones 1010 a 1111 no se usan.
> - **Gray:** entre un número y el siguiente cambia un solo bit. Se usa en encoders y en el orden de los mapas de Karnaugh. Se obtiene así: `gₙ = bₙ` y `gᵢ = bᵢ₊₁ ⊕ bᵢ`.
> - **ASCII:** 7 bits por carácter. 'A' = 65 = `1000001`; '0' = 48 = `0110000`.

| Dec | Binario | Gray | BCD |
|:-:|:-:|:-:|:-:|
| 0 | 0000 | 0000 | 0000 0000 |
| 1 | 0001 | 0001 | 0000 0001 |
| 2 | 0010 | 0011 | 0000 0010 |
| 3 | 0011 | 0010 | 0000 0011 |
| 4 | 0100 | 0110 | 0000 0100 |
| 5 | 0101 | 0111 | 0000 0101 |
| 6 | 0110 | 0101 | 0000 0110 |
| 7 | 0111 | 0100 | 0000 0111 |
| 8 | 1000 | 1100 | 0000 1000 |
| 9 | 1001 | 1101 | 0000 1001 |
| 10 | 1010 | 1111 | 0001 0000 |
| 11 | 1011 | 1110 | 0001 0001 |
| 12 | 1100 | 1010 | 0001 0010 |
| 13 | 1101 | 1011 | 0001 0011 |
| 14 | 1110 | 1001 | 0001 0100 |
| 15 | 1111 | 1000 | 0001 0101 |

## 5. Compuertas lógicas

> [!NOTE]
> **Definición.** Una **compuerta** es un circuito que realiza una operación lógica elemental sobre una o más entradas binarias y entrega una salida.

| Compuerta | Función | Se lee |
|:--|:-:|:--|
| **AND** | `A · B` | Vale 1 solo si todas las entradas valen 1 |
| **OR** | `A + B` | Vale 1 si alguna entrada vale 1 |
| **NOT** | `A′` | Invierte la entrada |
| **NAND** | `(A · B)′` | AND con la salida negada |
| **NOR** | `(A + B)′` | OR con la salida negada |
| **XOR** | `A ⊕ B` | Vale 1 si las entradas son diferentes (cuenta impar de unos) |
| **XNOR** | `(A ⊕ B)′` | Vale 1 si las entradas son iguales: detector de igualdad |

## 6. Tabla de verdad y hoja de datos del 74LS

Una **tabla de verdad** lista todas las combinaciones de entrada (2ⁿ filas para *n* entradas) y la salida que produce el circuito en cada una. Se escribe siguiendo el orden del conteo binario.

Los circuitos integrados de la familia **74LS** traen varias compuertas en un mismo encapsulado de 14 pines. En la hoja de datos se busca el diagrama de pines, la tabla de función (verdad), los voltajes de alimentación y los niveles lógicos.

| Circuito | Función | Compuertas |
|:--|:--|:--|
| **74LS08** | AND | 4 de 2 entradas |
| **74LS32** | OR | 4 de 2 entradas |
| **74LS04** | NOT (inversor) | 6 |
| **74LS00** | NAND | 4 de 2 entradas |
| **74LS02** | NOR | 4 de 2 entradas |
| **74LS86** | XOR | 4 de 2 entradas |
| **74LS266** | XNOR (colector abierto) | 4 de 2 entradas |

> [!WARNING]
> En la familia 74LS el pin **14 es Vcc (+5 V)** y el pin **7 es GND** en casi todos los integrados de 14 pines. Si los inviertes, el chip se calienta y se daña. Verifica siempre con la hoja de datos del fabricante, porque las excepciones existen.

## 7. Profundizando la clase

### 7.1 Las potencias de 2 que debes saber de memoria

| 2⁰ | 2¹ | 2² | 2³ | 2⁴ | 2⁵ | 2⁶ | 2⁷ | 2⁸ | 2⁹ | 2¹⁰ |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| 1 | 2 | 4 | 8 | 16 | 32 | 64 | 128 | 256 | 512 | 1024 |

Con ellas conviertes de binario a decimal sumando solo las posiciones que valen 1, y sabes cuántos valores distintos caben en *n* bits (2ⁿ).

> **Ejemplo resuelto**
>
> - **Binario a decimal:** `11010110` = 128 + 64 + 16 + 4 + 2 = **214**. Agrupado de 4 en 4 es `1101 0110` = D6 en hexadecimal; comprueba: 13·16 + 6 = 214.
> - **Decimal a hexadecimal:** 478 ÷ 16 = 29 residuo 14 (E); 29 ÷ 16 = 1 residuo 13 (D); 1 ÷ 16 = 0 residuo 1. Leído de abajo hacia arriba: **1DE₁₆**.

### 7.2 Niveles lógicos reales en la familia 74LS

Un 0 y un 1 lógicos son rangos de voltaje, no un valor exacto. Esto da margen frente al ruido eléctrico.

| Parámetro | Significado | Valor típico 74LS |
|:--|:--|:--|
| V<sub>CC</sub> | Alimentación | 5 V (4,75 a 5,25 V) |
| V<sub>IL</sub> máx. | Mayor voltaje de entrada que se lee como 0 | 0,8 V |
| V<sub>IH</sub> mín. | Menor voltaje de entrada que se lee como 1 | 2,0 V |
| V<sub>OL</sub> máx. | Voltaje de salida en nivel bajo | 0,5 V |
| V<sub>OH</sub> mín. | Voltaje de salida en nivel alto | 2,7 V |

> [!NOTE]
> Entre 0,8 V y 2,0 V una entrada queda en zona indefinida y la compuerta puede responder de forma impredecible. Confirma siempre estos valores en la hoja de datos de tu fabricante.

**Lógica positiva y negativa:** en lógica positiva el voltaje alto es el 1 (lo usual). En lógica negativa es al revés. Una compuerta AND en lógica positiva se comporta como OR en lógica negativa. Todo el curso usa lógica positiva.

### 7.3 Resumen de las siete compuertas

| A | B | AND | OR | NAND | NOR | XOR | XNOR | NOT A |
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
| 0 | 0 | 0 | 0 | 1 | 1 | 0 | 1 | 1 |
| 0 | 1 | 0 | 1 | 1 | 0 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 | 1 | 0 | 1 | 0 | 0 |
| 1 | 1 | 1 | 1 | 0 | 0 | 0 | 1 | 0 |

**Compuertas de más entradas.** Una AND de 3 entradas vale 1 solo en la fila 111. Una OR de 3 vale 0 solo en la fila 000. Una XOR de 3 entradas vale 1 cuando la cantidad de unos es impar: por eso se usa como generador de paridad.

### 7.4 Más códigos con ejemplos

> **Ejemplo resuelto**
>
> - **Binario a Gray:** el bit más significativo se copia y cada bit siguiente es la XOR entre el bit de arriba y el suyo. `1011` → 1, 1⊕0=1, 0⊕1=1, 1⊕1=0 → **1110**.
> - **Gray a binario:** se copia el primer bit y cada bit siguiente es la XOR entre el binario ya obtenido (el bit anterior) y el bit Gray. `1110` → 1, 1⊕1=0, 0⊕1=1, 1⊕0=1 → **1011**.
> - **ASCII de la palabra "Hola":** H = 72 = `01001000`, o = 111 = `01101111`, l = 108 = `01101100`, a = 97 = `01100001`.
> - **Suma en BCD (27 + 15):** `0010 0111 + 0001 0101`. Las unidades dan `0111 + 0101 = 1100`, que no es un dígito BCD válido (mayor que 9), entonces se suma `0110` de corrección: `1100 + 0110 = 1 0010`. Queda dígito 2 con acarreo 1. Las decenas: `0010 + 0001 + 1 = 0100`. Resultado: **`0100 0010` = 42**.

### 7.5 De un enunciado a una compuerta

> **Ejemplo resuelto: luz de pasillo con dos interruptores.** La lámpara cambia de estado cada vez que se acciona cualquiera de los dos interruptores. Con A y B como interruptores, la lámpara se enciende cuando *exactamente uno* está activo: `F = A ⊕ B`. Es una XOR. Si se quisiera que encienda cuando están en la misma posición, sería una XNOR.

### 7.6 Ejercicios de práctica con respuesta

<details><summary><b>a)</b> Convierte 37₁₀ a binario.</summary>

`100101` (32 + 4 + 1).
</details>

<details><summary><b>b)</b> Convierte 3F₁₆ a binario y a decimal.</summary>

`0011 1111` = 63.
</details>

<details><summary><b>c)</b> Escribe −6 en complemento a dos con 4 bits.</summary>

6 = `0110` → `1001` → +1 = `1010`.
</details>

<details><summary><b>d)</b> ¿Cuál es el BCD de 83?</summary>

`1000 0011`.
</details>

## 8. Preguntas clave para entender el tema

<details><summary><b>¿Por qué un computador usa solo dos estados y no diez?</b></summary>

Dos niveles de voltaje se distinguen con facilidad aunque haya ruido eléctrico; con más niveles los rangos se acercan y habría más errores.
</details>

<details><summary><b>¿Para qué sirven el octal y el hexadecimal si el computador solo usa binario?</b></summary>

Son una forma corta de escribir binario para las personas: cada dígito octal agrupa 3 bits y cada dígito hexadecimal agrupa 4.
</details>

<details><summary><b>¿Por qué el complemento a dos es mejor que usar solo un bit de signo?</b></summary>

Porque la misma suma binaria funciona para positivos y negativos, y el cero tiene una sola representación. Esto permite restar con el mismo circuito sumador.
</details>

<details><summary><b>¿Cuándo conviene Gray y cuándo BCD?</b></summary>

Gray cuando se necesita que entre valores consecutivos cambie un solo bit (encoders, mapas de Karnaugh). BCD cuando se quiere mostrar dígitos decimales, como en un display.
</details>

<details><summary><b>¿Cómo sé cuántas filas lleva una tabla de verdad?</b></summary>

2ⁿ, donde *n* es el número de entradas. Con 3 entradas son 8 filas y con 4 son 16.
</details>

<details><summary><b>¿Qué distingue a una XOR de una OR?</b></summary>

La OR también vale 1 cuando las dos entradas son 1; la XOR vale 0 en ese caso porque solo detecta que las entradas son diferentes.
</details>

<details><summary><b>¿Qué debo buscar primero en la hoja de datos de un 74LS?</b></summary>

El diagrama de pines (dónde están V<sub>CC</sub> y GND y las entradas y salidas de cada compuerta) y la tabla de función.
</details>

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

# Semana 3 · Mapas de Karnaugh y universalidad NAND/NOR

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

# Glosario

| Término | Significado |
|:--|:--|
| **Acarreo (carry)** | Bit que pasa a la columna siguiente cuando una suma excede lo que cabe en un bit. |
| **Bit** | Dígito binario, 0 o 1. |
| **Circuito combinacional** | Circuito cuya salida depende solo del valor actual de las entradas. |
| **Complemento a dos** | Representación de números con signo; se obtiene invirtiendo bits y sumando 1. |
| **Condición indiferente** | Combinación de entrada cuya salida no importa; se marca X. |
| **De Morgan** | Pareja de teoremas que convierten ANDs en ORs y viceversa al negar. |
| **Implicante primo** | Grupo de unos en un mapa que no cabe dentro de otro grupo mayor. |
| **Maxitérmino** | Suma de todas las variables que vale 0 en una sola fila. |
| **Minitérmino** | Producto de todas las variables que vale 1 en una sola fila. |
| **Tabla de verdad** | Lista de todas las combinaciones de entrada con la salida que produce cada una. |
| **Universalidad** | Propiedad de NAND y NOR: con una sola se construye cualquier función. |

---

<div align="center">

*Diseño de Circuitos · Erick Andrade Montero · Universidad Nacional de Loja*

</div>
