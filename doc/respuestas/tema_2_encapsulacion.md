<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En POO, ¿qué buscan la encapsulación y la ocultación de información? Ventajas

### Respuesta

La **encapsulación** y la **ocultación de información** buscan separar claramente qué partes internas de un objeto son visibles desde el exterior y cuáles no. El objetivo principal es proteger el estado interno del objeto para que solo pueda modificarse de forma controlada, a través de métodos bien definidos.

Al ocultar los detalles internos, se evita que otras partes del programa dependan de cómo está implementada una clase. Esto permite cambiar la implementación sin afectar al código que la utiliza, siempre que se mantenga la interfaz pública.

Entre las ventajas se encuentran una mayor robustez del programa, menor acoplamiento entre clases, mejor mantenimiento del código y reducción de errores derivados de modificaciones no controladas del estado de los objetos.

Prof:
Encapsulación, tiene que ver con "Protección". He juntado estado y comportamiento en un artefacto (clase), y ahora puedo ocultar ciertas partes del exterior.
- Evito estados no validos de mas objetos.
- Evito dependencias desde fuera que no quiero.

---

## 2. ¿Qué es la interfaz pública de una clase? Relación con la ocultación

### Respuesta

La **interfaz pública** de una clase está formada por el conjunto de métodos y atributos accesibles desde fuera de la clase. Representa el “contrato” que la clase ofrece a otros componentes del programa.

La ocultación de información permite que solo esta interfaz sea visible, mientras que los detalles internos quedan escondidos. De esta forma, quien usa la clase no necesita conocer cómo funciona internamente, solo qué servicios ofrece.

Esta separación mejora la claridad del diseño y facilita la reutilización de clases sin riesgo de uso incorrecto.

Prof:
Interfaz pública -> los miembros que se ven desde fuera, es decir, los que no están ocultos.

---

## 3. Diseño cuidadoso de la interfaz pública

### Respuesta

La interfaz pública debe diseñarse con cuidado porque una vez que es utilizada por otras partes del programa, cambiarla puede resultar costoso y provocar errores en cascada. Cualquier modificación puede obligar a cambiar mucho código dependiente.

No suele ser fácil cambiar una interfaz pública sin romper compatibilidad. Por ello, es preferible exponer solo lo estrictamente necesario y mantener los detalles internos ocultos desde el principio.

Un diseño conservador de la interfaz favorece la estabilidad y la evolución del software a largo plazo.

Prof:
La interfaz oública si se cambia tiene más consecuencias que cualquier cambio en la parte oculta

---

## 4. Invariantes de clase y ocultación

### Respuesta

Las **invariantes de clase** son condiciones que siempre deben cumplirse para que un objeto sea válido. Por ejemplo, que una coordenada tenga un valor coherente o que un contador nunca sea negativo.

La ocultación de información ayuda a mantener estas invariantes, ya que evita que el estado interno se modifique directamente desde fuera de la clase. Solo los métodos internos pueden cambiar los datos.

De este modo, se garantiza que cualquier modificación pasa por controles definidos por la propia clase.

Prof:
Invariantes de clase: condicion que los objetos de esa clase deben cumplir para ser válidos durante toda su vida.
---

## 5. Ejemplo de clase `Punto` con encapsulación

### Respuesta

```java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

La interfaz pública está formada por el constructor y el método `calcularDistanciaAOrigen`.
`public` indica que un miembro es accesible desde cualquier clase.
`private` indica que solo es accesible desde la propia clase, ocultando la información interna.

---

## 6. ¿A quién se aplican `public` y `private` en Java?

### Respuesta

En Java, los modificadores `public` y `private` pueden aplicarse a **clases, atributos, métodos y constructores**. No todos los modificadores son válidos para todos los elementos.

`public` permite el acceso desde cualquier parte del programa. `private` restringe el acceso exclusivamente al interior de la clase donde se declara.

Este mecanismo es la base de la encapsulación en Java.

Prof:
- Public: clases, atributos y métodos
- Private: clases internas(no las estamos viendo), atributos y métodos.

---

## 7. Tipos de visibilidad

### Respuesta

Además de `public` y `private`, existen otros tipos de visibilidad. En Java existen cuatro niveles: `public`, `private`, `protected` y visibilidad por defecto (package-private).

`protected` permite acceso desde clases del mismo paquete o subclases. En otros lenguajes existen mecanismos similares, aunque los nombres y reglas pueden variar.

Esto permite un control más fino sobre qué partes del código pueden acceder a ciertos miembros.

Prof:
- Protected, solo se ve desde "subclases". (Las veremos en el tema de herencia).
- "package.private" o sin modificador, solo se ve desde el paquete.
---

## 8. Acceso a miembros privados entre instancias

### Respuesta

Los miembros privados están ocultos para **otras clases**, pero **no para otras instancias de la misma clase**. Una instancia puede acceder a los miembros privados de otra instancia si lo hace desde un método de la misma clase.

```java
public double calcularDistanciaAPunto(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

Esto es posible porque el acceso se realiza desde el código de la clase `Punto`, no desde una clase externa.

Prof:
- La a), está oculto para código de otras clases.
---

## 9. Getters y setters

### Respuesta

Los métodos **getter** permiten leer el valor de un atributo privado, mientras que los **setter** permiten modificarlo de forma controlada.

Estos métodos actúan como intermediarios entre el exterior y el estado interno del objeto. Permiten validar valores, mantener invariantes o ejecutar lógica adicional.

Son una herramienta fundamental para aplicar encapsulación sin perder flexibilidad.

prof:
"getter" y "setter" permiten dar acceso a atributos privados para obtener su valor o cambiarlo.

---

## 10. “Seguridad” y ocultación de información

### Respuesta

Cuando se habla de seguridad en este contexto, no se refiere a evitar que un programa sea hackeado. Se trata de **seguridad lógica y estructural del código**.

La ocultación evita usos incorrectos de los objetos, protege invariantes y reduce errores por manipulaciones indebidas.

Es una seguridad orientada a la calidad y robustez del software.

Prof:
- No, esto no es ciberseguridad, es facilitar una programación con menos bugs.

---

## 11. Miembros de instancia y de clase

### Respuesta

Un **miembro de instancia** pertenece a cada objeto creado, mientras que un **miembro de clase** pertenece a la clase en sí y es compartido por todas las instancias.

Los miembros de clase también pueden ocultarse usando `private`. La encapsulación se aplica igualmente a ambos tipos de miembros.

En Java, los miembros de clase se indican con la palabra clave `static`.

Prof:
- Miembro de clase -> no asociado a ninguna instancia; es compartido por todas las instancias.
- Miembro de instancia -> está asociado a cada instancia; no son compartidos.

---

## 12. Constructores privados

### Respuesta

Tiene sentido que los constructores sean privados en ciertos casos, por ejemplo cuando se quiere impedir la creación directa de objetos.

Esto se utiliza en patrones como el **Singleton** o en clases con métodos factoría. El objetivo es controlar cómo y cuántas instancias se crean.

No es lo habitual, pero es una herramienta válida de encapsulación.

Prof:
- A veces:
    - Un constructor auxiliar oculto, llamado desde otros constructores públicos.
    - Cuando usan métodos factoría.
    - Cuando quiero controlar el nº de instancias.

---

## 13. Miembros de clase en `Punto`

### Respuesta

```java
public class Punto {
    private double x, y;
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
}
```

Los miembros de clase se indican con `static`.

---

## 14. Método factoría

### Respuesta

```java
public static Punto crearPuntoRedondeado(double x, double y) {
    return new Punto(Math.round(x), Math.round(y));
}
```

Se ha usado `static` porque el método pertenece a la clase y no a una instancia concreta.

---

## 15. Uso de array interno

### Respuesta

```java
public class Punto {
    private double[] coord = new double[2];

    public Punto(double x, double y) {
        coord[0] = x;
        coord[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coord[0] * coord[0] + coord[1] * coord[1]);
    }
}
```

La interfaz pública se mantiene igual, aunque la implementación interna cambia.

---

## 16. ¿Atributos públicos o privados?

### Respuesta

Aunque existan getters y setters, no es recomendable hacer los atributos públicos. Mantenerlos privados permite cambiar la implementación sin afectar a la interfaz.

La convención habitual es declarar los atributos como `private`. Esto está directamente relacionado con la protección de las invariantes de clase.

Permite añadir validaciones y lógica adicional en el futuro.

Prof:
Si los hago públicos:
- Para poder garantizar la invariante de clase.
- Para poder cambiar la representación interna.
- Convención es:
 - Atributos siempre privados y emplear métodos de acceso.

---

## 17. Clases inmutables

### Respuesta

Una clase es **inmutable** cuando su estado no puede cambiar tras su creación. No existen métodos modificadores.

Un **método modificador** es aquel que cambia el estado del objeto, y no siempre es un setter. La inmutabilidad ofrece ventajas como mayor seguridad, simplicidad y facilidad para trabajar en entornos concurrentes.

Prof:
Inumatable -> su estado no cambia.
Modificador -> cualquier método que cambia el estado interno, por ejemplo: un setter.
Las clases inmutables tienen ventajas -> no hacer clases mutables por defecto.
---

## 18. ¿Setters siempre?

### Respuesta

No es recomendable incluir setters siempre por convención. Exponer setters innecesarios rompe la encapsulación y puede poner en riesgo las invariantes.

Solo deben incluirse cuando modificar el estado tenga sentido desde el punto de vista del diseño. Menos interfaz pública suele implicar mejor diseño.

Prof:
Por convención no, porque sino solo haría clases matuables.

---

## 19. `String` en Java

### Respuesta

La clase `String` en Java es **inmutable**. Al concatenar cadenas, se crea un nuevo objeto `String`.

Si se necesita concatenar muchas veces, es preferible usar `StringBuilder` o `StringBuffer`, que son mutables y más eficientes.

Prof:
String es inmutable.
```java
String eltitulo = libro.getTitulo();
elTitulo.setCharAt(0,'A'); // no funciona
elTitulo.substring(0,7);
elTitulo.append("mas texto"); (StringBuilder, se usa para usar Strings muy largas, encadenandolas mediante .append)
```
---

## 20. Comparación de objetos

### Respuesta

Los objetos pueden compararse por **identidad** o por **contenido**. En Java, `==` compara identidad, no contenido.

El método `equals` compara contenido. Por defecto, se comporta como `==`, pero suele redefinirse. Las cadenas deben compararse siempre con `equals`, no con `==`.

Prof:
A) Por identidad -> mismo objeto en memoria.
B) Por contenido -> Mismo estado (valor en sus atributos).

String s1 = new String("hola");
String s2 = new String("hola);

if (s1==s2) --> devuelve false porque s1 y s2 aunque ambos tengan el mismo contenido, tienen distinta dirección de memoria
if (s1.equals(s2))-> devuelve true, porque es la forma correcta para comparar objetos.
equals: por defecto hace comparación por identidad(==), excepto en clases concretas donde se implementa una comparación por contenido, por ejemplo en String.
---

## 21. Clases wrapper

### Respuesta

Las clases **wrapper** encapsulan tipos primitivos en objetos, como `Integer` para `int`. En Java, este proceso puede ser automático mediante *autoboxing*.

Permiten usar tipos primitivos en contextos donde se requieren objetos. No todos los lenguajes tienen tipos primitivos ni necesitan wrappers.

Prof:
- Ocurren en lenguajes que tienen tipos primitivos, por ejemplo Java.
- Otros lenguajes no tienen tipos primitivos, como Python.
    - int <=> Integer
    - float <=> Float
    - char <=> Character
- Añadirle comportamiento
- Poder usarlos en contestos donde se necesitan objetos. <List>.

```java
Integer i = 7; // Autoboxing
- Integer i = new Integer(7);

int j = i; // Unboxing
int j = i.intValue();
```
---

## 22. Tipos enumerados

### Respuesta

Un tipo enumerado representa un conjunto fijo de valores posibles. En Java, un `enum` es una clase especial.

Los enumerados permiten encapsular datos y comportamiento, evitando valores inválidos y mejorando la claridad del código.

Prof:
Enumerado es un tipo con un número determinado de valores posibles.
En Java un enumerado es una clase, cuyas instancias son finitas, conocidas de antemano, tienen un nombre cada una (valor de enumerado).

private TipoIVA (double factor){
    this.factr = factor;
}

---

## 23. Enumerado `Mes`

### Respuesta

```java
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private int dias;
    private int orden;

    Mes(int dias, int orden) {
        this.dias = dias;
        this.orden = orden;
    }

    public int getDias() {
        return dias;
    }

    public int getOrden() {
        return orden;
    }

    public boolean esDePrimavera(boolean norte) {
        return norte ? (orden >= 3 && orden <= 5) : (orden >= 9 && orden <= 11);
    }

    public boolean esDeVerano(boolean norte) {
        return norte ? (orden >= 6 && orden <= 8) : (orden == 12 || orden <= 2);
    }

    public boolean esDeOtono(boolean norte) {
        return norte ? (orden >= 9 && orden <= 11) : (orden >= 3 && orden <= 5);
    }

    public boolean esDeInvierno(boolean norte) {
        return norte ? (orden == 12 || orden <= 2) : (orden >= 6 && orden <= 8);
    }
}
```


---

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta

En Java, un enumerado se comporta como una clase especial que permite definir un conjunto fijo de constantes. Al utilizar atributos privados como `dias` y `orden`, se aplica el principio de encapsulación, asegurando que esta información solo pueda ser consultada a través de métodos públicos (getters) y no modificada desde el exterior.

El uso de un constructor en un `enum` permite que cada instancia (como `ENERO` o `FEBRERO`) se inicialice con valores específicos de forma inmutable. Esto es mucho más robusto que el uso de constantes enteras en lenguajes como C, ya que se garantiza la seguridad de tipos y se agrupa el estado y el comportamiento en una misma entidad.

```java
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private int dias;
    private int orden;

    // Constructor del enumerado
    Mes(int dias, int orden) {
        this.dias = dias;
        this.orden = orden;
    }

    public int getDias() {
        return dias;
    }

    public int getOrden() {
        return orden;
    }
}

```

## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

La implementación de la lógica estacional dentro del enumerado permite centralizar el conocimiento sobre el calendario en la propia clase `Mes`. Mediante el uso del parámetro booleano `norte`, se puede realizar un cálculo condicional que alterna los rangos de los meses según la inclinación del hemisferio, aprovechando el atributo `orden` para las comparaciones lógicas.

Este enfoque demuestra la potencia de los enumerados en Java frente a otros lenguajes: no solo actúan como etiquetas, sino que pueden contener lógica compleja. Al utilizar operadores ternarios o estructuras condicionales, se determina si el mes actual pertenece a una estación específica, facilitando la legibilidad del código externo que solo necesita llamar al método correspondiente.

```java
    public boolean esDePrimavera(boolean norte) {
        return norte ? (orden >= 3 && orden <= 5) : (orden >= 9 && orden <= 11);
    }

    public boolean esDeVerano(boolean norte) {
        return norte ? (orden >= 6 && orden <= 8) : (orden == 12 || orden <= 2);
    }

    public boolean esDeOtono(boolean norte) {
        return norte ? (orden >= 9 && orden <= 11) : (orden >= 3 && orden <= 5);
    }

    public boolean esDeInvierno(boolean norte) {
        return norte ? (orden == 12 || orden <= 2) : (orden >= 6 && orden <= 8);
    }

```

---
