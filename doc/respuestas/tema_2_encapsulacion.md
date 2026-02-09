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

---

## 2. ¿Qué es la interfaz pública de una clase? Relación con la ocultación

### Respuesta

La **interfaz pública** de una clase está formada por el conjunto de métodos y atributos accesibles desde fuera de la clase. Representa el “contrato” que la clase ofrece a otros componentes del programa.

La ocultación de información permite que solo esta interfaz sea visible, mientras que los detalles internos quedan escondidos. De esta forma, quien usa la clase no necesita conocer cómo funciona internamente, solo qué servicios ofrece.

Esta separación mejora la claridad del diseño y facilita la reutilización de clases sin riesgo de uso incorrecto.

---

## 3. Diseño cuidadoso de la interfaz pública

### Respuesta

La interfaz pública debe diseñarse con cuidado porque una vez que es utilizada por otras partes del programa, cambiarla puede resultar costoso y provocar errores en cascada. Cualquier modificación puede obligar a cambiar mucho código dependiente.

No suele ser fácil cambiar una interfaz pública sin romper compatibilidad. Por ello, es preferible exponer solo lo estrictamente necesario y mantener los detalles internos ocultos desde el principio.

Un diseño conservador de la interfaz favorece la estabilidad y la evolución del software a largo plazo.

---

## 4. Invariantes de clase y ocultación

### Respuesta

Las **invariantes de clase** son condiciones que siempre deben cumplirse para que un objeto sea válido. Por ejemplo, que una coordenada tenga un valor coherente o que un contador nunca sea negativo.

La ocultación de información ayuda a mantener estas invariantes, ya que evita que el estado interno se modifique directamente desde fuera de la clase. Solo los métodos internos pueden cambiar los datos.

De este modo, se garantiza que cualquier modificación pasa por controles definidos por la propia clase.

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

---

## 7. Tipos de visibilidad

### Respuesta

Además de `public` y `private`, existen otros tipos de visibilidad. En Java existen cuatro niveles: `public`, `private`, `protected` y visibilidad por defecto (package-private).

`protected` permite acceso desde clases del mismo paquete o subclases. En otros lenguajes existen mecanismos similares, aunque los nombres y reglas pueden variar.

Esto permite un control más fino sobre qué partes del código pueden acceder a ciertos miembros.

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

---

## 9. Getters y setters

### Respuesta

Los métodos **getter** permiten leer el valor de un atributo privado, mientras que los **setter** permiten modificarlo de forma controlada.

Estos métodos actúan como intermediarios entre el exterior y el estado interno del objeto. Permiten validar valores, mantener invariantes o ejecutar lógica adicional.

Son una herramienta fundamental para aplicar encapsulación sin perder flexibilidad.

---

## 10. “Seguridad” y ocultación de información

### Respuesta

Cuando se habla de seguridad en este contexto, no se refiere a evitar que un programa sea hackeado. Se trata de **seguridad lógica y estructural del código**.

La ocultación evita usos incorrectos de los objetos, protege invariantes y reduce errores por manipulaciones indebidas.

Es una seguridad orientada a la calidad y robustez del software.

---

## 11. Miembros de instancia y de clase

### Respuesta

Un **miembro de instancia** pertenece a cada objeto creado, mientras que un **miembro de clase** pertenece a la clase en sí y es compartido por todas las instancias.

Los miembros de clase también pueden ocultarse usando `private`. La encapsulación se aplica igualmente a ambos tipos de miembros.

En Java, los miembros de clase se indican con la palabra clave `static`.

---

## 12. Constructores privados

### Respuesta

Tiene sentido que los constructores sean privados en ciertos casos, por ejemplo cuando se quiere impedir la creación directa de objetos.

Esto se utiliza en patrones como el **Singleton** o en clases con métodos factoría. El objetivo es controlar cómo y cuántas instancias se crean.

No es lo habitual, pero es una herramienta válida de encapsulación.

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

---

## 17. Clases inmutables

### Respuesta

Una clase es **inmutable** cuando su estado no puede cambiar tras su creación. No existen métodos modificadores.

Un **método modificador** es aquel que cambia el estado del objeto, y no siempre es un setter. La inmutabilidad ofrece ventajas como mayor seguridad, simplicidad y facilidad para trabajar en entornos concurrentes.

---

## 18. ¿Setters siempre?

### Respuesta

No es recomendable incluir setters siempre por convención. Exponer setters innecesarios rompe la encapsulación y puede poner en riesgo las invariantes.

Solo deben incluirse cuando modificar el estado tenga sentido desde el punto de vista del diseño. Menos interfaz pública suele implicar mejor diseño.

---

## 19. `String` en Java

### Respuesta

La clase `String` en Java es **inmutable**. Al concatenar cadenas, se crea un nuevo objeto `String`.

Si se necesita concatenar muchas veces, es preferible usar `StringBuilder` o `StringBuffer`, que son mutables y más eficientes.

---

## 20. Comparación de objetos

### Respuesta

Los objetos pueden compararse por **identidad** o por **contenido**. En Java, `==` compara identidad, no contenido.

El método `equals` compara contenido. Por defecto, se comporta como `==`, pero suele redefinirse. Las cadenas deben compararse siempre con `equals`, no con `==`.

---

## 21. Clases wrapper

### Respuesta

Las clases **wrapper** encapsulan tipos primitivos en objetos, como `Integer` para `int`. En Java, este proceso puede ser automático mediante *autoboxing*.

Permiten usar tipos primitivos en contextos donde se requieren objetos. No todos los lenguajes tienen tipos primitivos ni necesitan wrappers.

---

## 22. Tipos enumerados

### Respuesta

Un tipo enumerado representa un conjunto fijo de valores posibles. En Java, un `enum` es una clase especial.

Los enumerados permiten encapsular datos y comportamiento, evitando valores inválidos y mejorando la claridad del código.

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

