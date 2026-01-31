<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

Las cuatro características fundamentales de la programación orientada a objetos (POO) son **abstracción, encapsulamiento, herencia y polimorfismo**. Estas ideas permiten organizar los programas de forma más cercana a cómo se perciben los objetos del mundo real, facilitando el diseño y mantenimiento del software.

La **abstracción** consiste en quedarse con las características esenciales de un objeto y ocultar los detalles innecesarios. El **encapsulamiento** implica agrupar datos y funciones dentro de una misma estructura (la clase) y controlar el acceso a ellos. La **herencia** permite crear nuevas clases a partir de otras existentes, reutilizando código. El **polimorfismo** permite que un mismo método tenga diferentes comportamientos según el objeto que lo utilice.

---

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Existen muchos lenguajes que soportan programación orientada a objetos. Entre los más populares se encuentran **Java, C++, Python y C#**. Todos ellos permiten definir clases, crear objetos y aplicar los principios básicos de la POO.

Cada uno implementa estos conceptos con ligeras diferencias. Por ejemplo, Java y C# están muy centrados en clases, Python es más flexible y combina varios paradigmas, y C++ mezcla programación estructurada y orientada a objetos.

---

## 3. ¿Qué es la programación estructurada? ¿Qué es la programación modular?

### Respuesta

La **programación estructurada** es un paradigma anterior a la POO que organiza el código mediante estructuras de control claras como secuencias, decisiones (`if`) y bucles (`while`, `for`). Su objetivo es evitar saltos descontrolados en el programa y mejorar la legibilidad.

La **programación modular** se centra en dividir un programa grande en partes más pequeñas llamadas módulos o funciones. Cada módulo se encarga de una tarea concreta, lo que facilita el mantenimiento y la reutilización del código.

---

## 4. ¿Qué tres elementos definen a un objeto en POO?

### Respuesta

Un objeto se define por **estado, comportamiento e identidad**. Estos tres elementos permiten diferenciar un objeto de otro dentro de un programa.

El **estado** está formado por los datos que posee (atributos). El **comportamiento** está formado por las acciones que puede realizar (métodos). La **identidad** permite distinguir un objeto de otro, incluso si tienen los mismos valores en sus atributos.

---

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes OO manejan clases?

### Respuesta

Una **clase** es una plantilla o modelo que define cómo serán los objetos: qué datos tendrán y qué acciones podrán realizar. No es lo mismo que un objeto, sino la definición a partir de la cual se crean.

Un **objeto** es una **instancia** concreta de una clase, es decir, un elemento real creado a partir de esa plantilla. No todos los lenguajes orientados a objetos usan clases de la misma forma; algunos, como Java, están totalmente basados en clases, mientras que otros, como JavaScript, usan modelos basados en prototipos.

---

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la recolección de basura?

### Respuesta

En muchos lenguajes, los objetos se almacenan en una zona de memoria llamada **heap**. Las variables que los referencian suelen estar en otra zona, como la pila (stack). Sin embargo, el funcionamiento exacto depende del lenguaje y su implementación.

La **recolección de basura** es un mecanismo automático que libera la memoria de objetos que ya no se usan. Esto evita que el programador tenga que liberar la memoria manualmente.

---

## 7. ¿Qué es un método? ¿Qué es la sobrecarga de métodos?

### Respuesta

Un **método** es una función definida dentro de una clase que describe el comportamiento de sus objetos. Puede operar con los datos del objeto y realizar acciones.

La **sobrecarga de métodos** ocurre cuando existen varios métodos con el mismo nombre pero con distintos parámetros. El compilador decide cuál usar según los argumentos proporcionados.

---

## 8. Clase `Punto` en Java

### Respuesta

```java
class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

class Prueba {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        System.out.println(p.calculaDistanciaAOrigen());
    }
}
```

Se define una clase con dos atributos y un método. Luego se crea un objeto y se utiliza el método.

---

## 9. Punto de entrada, `static` y `final`

### Respuesta

El punto de entrada de un programa en Java es el método `main`. Es el primer método que se ejecuta al iniciar el programa.

La palabra clave **`static`** indica que un elemento pertenece a la clase y no a los objetos. No se usa solo en `main`, también en variables y métodos compartidos. **`final`** indica que algo no puede modificarse o redefinirse.

---

## 10. Compilar y ejecutar Java

### Respuesta

Para compilar se usa:
`javac NombreArchivo.java`
Para ejecutar:
`java NombreClase`

Java es un lenguaje compilado a un formato intermedio llamado **bytecode**. Este se ejecuta en la **máquina virtual de Java (JVM)**, que permite que el mismo programa funcione en distintos sistemas.

---

## 11. `new` y constructor

### Respuesta

La palabra **`new`** crea un objeto en memoria. Un **constructor** es un método especial que se ejecuta al crear el objeto.

```java
class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

---

## 12. `this`

### Respuesta

`this` es una referencia al propio objeto que está ejecutando el método. Se usa para diferenciar atributos del objeto de variables locales.

```java
this.x = x;
```

No todos los lenguajes usan la misma palabra, pero el concepto existe.

---

## 13. Método `distanciaA`

### Respuesta

```java
double distanciaA(Punto otro) {
    double dx = this.x - otro.x;
    double dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}
```

Calcula la distancia entre dos puntos.

---

## 14. Paso de parámetros

### Respuesta

En Java, los objetos se pasan por copia de la referencia. Se puede modificar el objeto, pero no cambiar la referencia original.

Los tipos primitivos como `int` se pasan por copia del valor. Los cambios dentro del método no afectan fuera.

---

## 15. `toString()`

### Respuesta

`toString()` devuelve una representación en texto del objeto. Existe en muchos lenguajes con otros nombres.

```java
public String toString() {
    return "(" + x + ", " + y + ")";
}
```

---

## 16. Clase vs `struct`

### Respuesta

Un `struct` en C agrupa datos, similar a los atributos de una clase. Sin embargo, no incluye métodos ni control de acceso.

Para ser como una clase, necesitaría funciones asociadas y mecanismos de encapsulación.

---

## 17. Emular clase en C

### Respuesta

```c
#include <math.h>

struct Punto {
    double x;
    double y;
};

double calculaDistanciaAOrigen(struct Punto p) {
    return sqrt(p.x * p.x + p.y * p.y);
}
```

Aquí no existe `this`; se pasa la estructura como parámetro explícito.
