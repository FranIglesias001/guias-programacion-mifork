# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

Para lograr una estructura de datos capaz de almacenar cualquier tipo de elemento sin utilizar los mecanismos modernos de genericidad, se puede recurrir al uso de la clase raíz de la jerarquía de objetos en Java: `Object`. Dado que cualquier clase en Java hereda implícita o explícitamente de `Object`, un array de este tipo es capaz de guardar referencias a instancias de cualquier clase.

A continuación, se presenta un ejemplo de una clase `Contenedor` en Java que encapsula un array primitivo de tipo `Object`. Esta estructura mantiene un índice para saber dónde insertar el siguiente elemento y ofrece un método básico para recuperar elementos por su posición.

Al utilizar este enfoque, es posible añadir tanto cadenas de texto como números (los cuales Java envuelve automáticamente en sus clases *wrapper* como `Integer` o `Double`) dentro de la misma instancia de la estructura, logrando así un comportamiento que acepta múltiples tipos.

```java
public class Contenedor {
    private Object[] elementos;
    private int contador;

    public Contenedor(int capacidad) {
        this.elementos = new Object[capacidad];
        this.contador = 0;
    }

    public void agregar(Object elemento) {
        if (contador < elementos.length) {
            elementos[contador++] = elemento;
        }
    }

    public Object obtener(int indice) {
        if (indice >= 0 && indice < contador) {
            return elementos[indice];
        }
        return null;
    }
}

```

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica?

### Respuesta

La programación genérica es un paradigma o estilo de programación que se centra en escribir algoritmos y estructuras de datos de manera independiente a los tipos de datos concretos sobre los que van a operar. El objetivo principal es la reutilización de código: escribir una única vez la lógica (por ejemplo, el mecanismo de inserción en una lista) y permitir que funcione de manera segura y eficiente con distintos tipos de datos (enteros, cadenas de texto, objetos complejos, etc.) sin necesidad de duplicar el código para cada uno.

El ejemplo anterior, basado en el uso de `Object` (o `void*` en C), representa un intento primigenio de lograr flexibilidad frente a los tipos, pero **no se considera un ejemplo verdadero o moderno de programación genérica**. Aunque estructuralmente permite almacenar diferentes tipos, lo hace a costa de perder la información del tipo original.

En la verdadera programación genérica, el lenguaje proporciona mecanismos para parametrizar los tipos, garantizando que, aunque la estructura sea genérica, las instancias concretas mantienen una estricta seguridad de tipos en tiempo de compilación. El enfoque con `Object` es más bien un polimorfismo basado en la herencia, también conocido como "type erasure" manual, que carece de las garantías fundamentales de la programación genérica propiamente dicha.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas.

### Respuesta

El principal problema de emplear `void*` o `Object` es la pérdida total de la seguridad de tipos (type safety) durante el tiempo de compilación. Cuando un dato se introduce en una estructura basada en `Object`, el compilador "olvida" su tipo original. Por lo tanto, si se crea un contenedor pensado exclusivamente para cadenas de texto, el compilador no mostrará ningún error si accidentalmente se inserta un número entero; ambas operaciones son válidas porque tanto `String` como `Integer` son subclases de `Object`.

Como consecuencia directa de esta pérdida de información, surge la necesidad obligatoria de realizar un *downcasting* (conversión explícita de tipos) al extraer los datos de la estructura. Al recuperar un elemento que el método devuelve como `Object`, el programador debe forzar su conversión al tipo esperado (por ejemplo, `(String) contenedor.obtener(0)`). Esta operación es propensa a errores humanos y oscurece la legibilidad del código.

Finalmente, si el *downcasting* se realiza incorrectamente (intentando convertir un `Integer` en un `String`), el error no se detectará hasta que el programa se esté ejecutando, provocando una excepción en tiempo de ejecución (como `ClassCastException` en Java). Los errores en tiempo de ejecución son mucho más difíciles de rastrear, depurar y prevenir que los errores detectados tempranamente por el compilador, lo que hace que las bases de código construidas sobre `Object` o `void*` sean inherentemente más frágiles.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**?

### Respuesta

Los parámetros de tipo son el mecanismo fundamental que introducen los lenguajes de programación modernos para implementar una programación genérica robusta y segura. De la misma manera que un método tradicional utiliza parámetros formales para recibir diferentes valores al ser invocado, una clase, interfaz o método genérico utiliza "parámetros de tipo" para recibir diferentes **tipos de datos** en el momento de su instanciación o invocación.

Sintácticamente, los parámetros de tipo suelen representarse entre símbolos de menor y mayor (como `<T>`, `<E>`, `<K, V>`). Estas variables de tipo actúan como marcadores de posición o plantillas dentro de la definición de la clase o método. Cuando el código se utiliza posteriormente, estos marcadores se sustituyen por tipos de datos concretos y reales proporcionados por el programador (por ejemplo, `String` o `Integer`).

La introducción de los parámetros de tipo soluciona los problemas del polimorfismo basado en `Object`. Al especificar el tipo exacto que contendrá una estructura de datos, el compilador adquiere la capacidad de realizar un análisis exhaustivo. Esto permite rechazar inserciones de tipos incompatibles en tiempo de compilación y elimina la necesidad de realizar *downcasting* al recuperar los elementos, ya que el compilador conoce de antemano el tipo exacto que se está devolviendo.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En Java, el sistema de genericidad se conoce como "Generics". Al utilizar colecciones de la biblioteca estándar, se especifica el tipo concreto entre ángulos. En el siguiente ejemplo, se instancia una lista para almacenar exclusivamente objetos de tipo `String`. Al recorrerla, se puede invocar directamente un método específico de la clase `String` (como `toUpperCase()`) sin necesidad de realizar ningún *cast*, demostrando que el compilador asegura el tipo.

```java
import java.util.ArrayList;
import java.util.List;

public class EjemploJava {
    public static void main(String[] args) {
        // Instanciación con Generics
        List<String> listaNombres = new ArrayList<>();
        listaNombres.add("Ada");
        listaNombres.add("Alan");

        // Recorrido seguro: cada elemento se trata como String
        for (String nombre : listaNombres) {
            System.out.println("Longitud del nombre: " + nombre.length());
        }
    }
}

```

En C++, el mecanismo equivalente se denomina "Templates" (plantillas). Se utiliza la clase `std::vector` de la Standard Template Library (STL). Al igual que en Java, el tipo de dato se especifica entre ángulos al declarar el vector. La iteración posterior garantiza que los elementos extraídos son estrictamente de tipo `std::string`, permitiendo invocar métodos propios de las cadenas de C++ con total seguridad.

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    // Instanciación con Templates
    std::vector<std::string> vectorNombres;
    vectorNombres.push_back("Ada");
    vectorNombres.push_back("Alan");

    // Recorrido seguro: cada elemento se trata como std::string
    for (const std::string& nombre : vectorNombres) {
        std::cout << "Longitud del nombre: " << nombre.length() << std::endl;
    }
    return 0;
}

```

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

Cuando se instancia una clase con parámetros de tipo, el compilador procesa estas directivas para garantizar la seguridad de tipos, pero el comportamiento subyacente difiere radicalmente dependiendo del lenguaje de programación. **No**, C++ y Java no hacen lo mismo bajo el capó; utilizan estrategias completamente opuestas para implementar la genericidad.

En Java, el mecanismo empleado se denomina **"type erasure"** (borrado de tipos). Para mantener la compatibilidad hacia atrás con versiones antiguas de Java que no poseían generics, el compilador realiza todas las comprobaciones de seguridad durante la compilación, pero posteriormente "borra" o elimina los parámetros de tipo en el bytecode generado. Todas las referencias a `<T>` se sustituyen por `Object` (o por el límite superior si está restringido), y el compilador inserta automáticamente los *casts* (conversiones) necesarios donde se recuperan los datos. Esto significa que en tiempo de ejecución, una `List<String>` y una `List<Integer>` son exactamente la misma clase (`ArrayList` pura).

Por el contrario, en C++, el mecanismo se denomina **"instanciación de plantillas"** (template instantiation) o monomorfización. Cuando el compilador de C++ encuentra el uso de un `std::vector<int>` y un `std::vector<std::string>`, genera código máquina completamente distinto e independiente para cada una de esas versiones. Es como si el compilador escribiera automáticamente dos clases diferentes. Esto produce un rendimiento en tiempo de ejecución sumamente eficiente (ya que no hay conversiones implícitas y se permite optimización a bajo nivel), pero a costa de aumentar el tamaño del archivo ejecutable final (fenómeno conocido como *code bloat*).

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`.

### Respuesta

Para definir una clase capaz de alojar dos valores que potencialmente pueden ser de tipos distintos, se requieren dos parámetros de tipo diferentes en la declaración de la clase. Habitualmente se utilizan convenciones como `T` (Type) y `U` (otro Type) o `K` (Key) y `V` (Value). Esta clase estructurará internamente los dos elementos garantizando la seguridad de tipos para ambos.

A continuación, se define la clase `Par<T, U>`, la cual incorpora atributos privados, un constructor para inicializarlos y métodos de acceso (getters). Posteriormente, se muestra una función estática que realiza un cálculo sobre un array y devuelve dos resultados empaquetados dentro de una instancia concreta de `Par`, en este caso utilizando `Double` para ambos tipos.

```java
public class Par<T, U> {
    private final T primero;
    private final U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}

class Estadistica {
    // Ejemplo de uso: función que retorna dos valores del mismo tipo (Double) empaquetados
    public static Par<Double, Double> calcularMediaYDesviacion(double[] datos) {
        if (datos == null || datos.length == 0) return new Par<>(0.0, 0.0);
        
        double suma = 0;
        for (double d : datos) suma += d;
        double media = suma / datos.length;
        
        double sumaCuadrados = 0;
        for (double d : datos) sumaCuadrados += Math.pow(d - media, 2);
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);
        
        return new Par<>(media, desviacion);
    }
}

```

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo.

### Respuesta

Los métodos genéricos permiten introducir parámetros de tipo cuyo alcance se limita exclusivamente a la ejecución de dicho método. Esto resulta especialmente útil para funciones de utilidad que no requieren que toda la clase sea genérica. A continuación, se presenta la comparación entre la versión sin generics y la versión con generics para el método `seleccionaUno`.

```java
import java.util.Random;

public class Utilidades {
    private static final Random random = new Random();

    // Versión SIN generics (usando Object)
    public static Object seleccionaUnoObject(Object a, Object b) {
        return random.nextBoolean() ? a : b;
    }

    // Versión CON generics
    public static <T> T seleccionaUnoGenerico(T a, T b) {
        return random.nextBoolean() ? a : b;
    }
    
    public static void main(String[] args) {
        // Uso de la versión Object
        String resultadoObj = (String) seleccionaUnoObject("Hola", "Adiós"); // (i) Requiere downcasting
        Object mezcla = seleccionaUnoObject("Texto", 42); // (ii) NO fuerza que sean del mismo tipo

        // Uso de la versión Genérica
        String resultadoGen = seleccionaUnoGenerico("Hola", "Adiós"); // (i) Evita downcasting
        // String error = seleccionaUnoGenerico("Texto", 42); // (ii) Error de compilación: fuerza el mismo tipo
    }
}

```

Respecto al **downcasting**, la versión con `Object` devuelve un `Object`, por lo que el programador está obligado a realizar una conversión explícita `(String)` para poder usar el resultado como texto, con el consiguiente riesgo de error. En la versión genérica `<T>`, el compilador infiere que si se pasan dos `String`, el valor de retorno será obligatoriamente un `String`, eliminando la necesidad de conversión.

Respecto a **forzar el mismo tipo**, la versión con `Object` acepta cualquier combinación de parámetros (por ejemplo, un `String` y un `Integer`), lo que puede carecer de sentido semántico según la lógica del programa. La versión genérica, al compartir la misma variable de tipo `T` para ambos argumentos, instruye al compilador para que rechace la llamada si los objetos no comparten una jerarquía de tipos compatible, detectando inconsistencias lógicas de inmediato.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

Sí, es posible y muy común establecer restricciones en los parámetros de tipo. Este concepto se denomina **"bounded type parameters"** (límites de tipo). Mediante la palabra clave `extends`, se le indica al compilador que un parámetro de tipo genérico debe ser obligatoriamente una subclase de una clase específica o implementar una interfaz concreta. Esto permite invocar de forma segura los métodos definidos en esa clase límite.

A continuación, se presentan las dos soluciones. La primera utiliza directamente la clase `Number` (sin generics). La segunda emplea generics con una restricción explícita (`<T Number extends>`).

```java
// Solución 1: Sin generics, usando Number directamente
public class PuntoNumber {
    private Number x, y;

    public PuntoNumber(Number x, Number y) {
        this.x = x; this.y = y;
    }
    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(PuntoNumber otro) {
        return Math.sqrt(Math.pow(this.x.doubleValue() - otro.x.doubleValue(), 2) +
                         Math.pow(this.y.doubleValue() - otro.y.doubleValue(), 2));
    }
}

// Solución 2: Con generics y restricción superior (Bounded Type Parameters)
public class PuntoGenerico<T extends Number> {
    private T x, y;

    public PuntoGenerico(T x, T y) {
        this.x = x; this.y = y;
    }
    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(PuntoGenerico<T> otro) {
        return Math.sqrt(Math.pow(this.x.doubleValue() - otro.x.doubleValue(), 2) +
                         Math.pow(this.y.doubleValue() - otro.y.doubleValue(), 2));
    }
}

```

Respecto al **"type erasure"**, el proceso de compilación de Java elimina la información genérica, pero tiene en cuenta los límites establecidos. En el caso de `<T Number extends>`, como el límite superior es `Number`, el compilador sustituye todas las apariciones de `T` por la clase `Number` en el bytecode resultante. Por lo tanto, tras la compilación, internamente ambas clases resultan prácticamente idénticas a nivel de atributos y firmas base, aunque el compilador gestiona inserciones de *casts* invisibles para la versión genérica al recuperar los datos.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

Al reflexionar sobre la mezcla de tipos en las coordenadas, la solución sin generics (usando atributos `Number`) es mucho más permisiva. Dado que el constructor acepta cualquier objeto que sea `Number`, es perfectamente posible instanciar un `PuntoNumber` pasando un `Integer` para `x` y un `Double` para `y`. Por el contrario, la solución con generics (`PuntoGenerico<T Number extends>`) refuerza el chequeo de tipos: al declarar, por ejemplo, `new PuntoGenerico<Integer>(5, 3.14)`, el compilador emitirá un error, forzando a que ambas coordenadas compartan estrictamente el mismo tipo concreto especificado en `T`.

En cuanto a los tipos de retorno, esta es la diferencia principal y la mayor ventaja de la solución genérica. En la clase `PuntoNumber`, el método `getX()` devuelve invariablemente un objeto de tipo genérico `Number`. Si el programador sabe que introdujo enteros y necesita operar con ellos como instancias de `Integer`, estará forzado a aplicar un *downcasting* explícito (e.g., `(Integer) punto.getX()`).

Sin embargo, en la clase `PuntoGenerico<T Number extends>`, el tipo de retorno se adapta dinámicamente. Si se instanció un `PuntoGenerico<Double>`, el método `getX()` devolverá directamente un `Double`. Esto elimina cualquier necesidad de conversiones por parte del usuario, reduce la verbosidad del código y traslada la responsabilidad de la coherencia de tipos al compilador, garantizando la robustez del programa.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.

### Respuesta

Para resolver el problema del chequeo en tiempo de ejecución (`instanceof`) y el consiguiente *downcasting*, se puede aplicar un patrón avanzado de generics en la interfaz. La idea es parametrizar la propia interfaz con el tipo concreto que la implementará. De esta manera, el método de la interfaz exigirá estrictamente un objeto del mismo tipo que la clase que lo implementa.

Al modificar la interfaz a `Punto<T>`, la clase `Punto2D` especificará que implementa `Punto<Punto2D>`. Esto obliga al compilador a firmar el método como `distanciaA(Punto2D p)`, eliminando cualquier ambigüedad desde la compilación.

```java
// Interfaz genérica
public interface Punto<T> { 
    public double distanciaA(T p); 
} 

// Implementación forzando T a ser Punto2D
public class Punto2D implements Punto<Punto2D> { 
    private final double x, y; 
    
    public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto2D p) { 
        // Ya no es necesario 'instanceof' ni '(Punto2D) p'
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2)); 
    } 
} 

// Implementación forzando T a ser Punto3D
public class Punto3D implements Punto<Punto3D> { 
    private final double x, y, z;
    
    public Punto3D(double x, double y, double z) {
        this.x = x; this.y = y; this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2) + Math.pow(z - p.z, 2));
    }
} 

```

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

Aunque `String` hereda de `Object`, una `List<String>` **no** es subtipo de una `List<Object>`. En Java, los tipos genéricos son diseñados deliberadamente para ser restrictivos y evitar problemas de seguridad de tipos. Si se permitiera esta asignación, se podría tomar una lista de cadenas, asignarla a una variable de lista de objetos, e insertar un `Integer` en ella. Al intentar leerla desde la referencia original de cadenas, el programa colapsaría.

Por el contrario, un array de `String[]` **sí** es subtipo de `Object[]` por decisiones históricas del diseño inicial de Java. Sin embargo, esto introduce una falla de seguridad de tipos en tiempo de diseño. Dado que el array `String[]` puede ser tratado como `Object[]`, el compilador permite asignarle un objeto `Integer`. No obstante, el array conoce su tipo real en memoria, por lo que esta asignación lanzará un error en tiempo de ejecución conocido como `ArrayStoreException`, revelando la fragilidad de esta aproximación comparada con las colecciones genéricas.

A partir de este comportamiento se definen tres conceptos fundamentales en los sistemas de tipos:

* **Invariante:** Ocurre en el caso de las listas genéricas en Java. `List<String>` no guarda ninguna relación de herencia con `List<Object>`, a pesar de que sus tipos internos sí la tengan. Un tipo es requerido de forma estricta.
* **Covariante:** Ocurre con los arrays en Java. Se preserva la dirección de la herencia: si `String` es hijo de `Object`, entonces el contenedor `String[]` es hijo del contenedor `Object[]`.
* **Contravariante:** Es la inversión de la jerarquía de herencia. Ocurriría si el sistema estableciera que un contenedor de `Object` es subtipo de un contenedor de `String` (útil en escenarios donde solo se consumen o escriben datos).

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un wildcard (comodín), representado por el símbolo `?`, es una herramienta en los genéricos de Java que representa un "tipo desconocido". Permite relajar la rigidez (invarianza) de los genéricos para crear referencias que puedan aceptar múltiples variantes de tipos parametrizados, recuperando así comportamientos covariantes o contravariantes de forma segura y controlada por el compilador.

La principal diferencia radica en su límite jerárquico. `List<? extends T>` establece un límite superior (covarianza): acepta colecciones de `T` o de cualquier subclase de `T`. Se utiliza cuando la lista es una estructura **productora** de datos (solo se va a leer de ella), ya que garantiza que todo lo extraído será al menos de tipo `T`, pero prohíbe las inserciones (salvo `null`) porque se desconoce el tipo exacto. Por otro lado, `List<? super T>` establece un límite inferior (contravarianza): acepta colecciones de `T` o de cualquier superclase de `T`. Se usa cuando la lista es **consumidora** (solo se va a escribir en ella), asegurando que siempre es seguro insertarle elementos de tipo `T` o sus subclases.

A continuación, se muestran ambos ejemplos aplicados para sumar números (lectura) y añadir enteros (escritura):

```java
import java.util.List;

public class EjemploWildcards {
    
    // (i) Uso de <? extends T> para LEER de la lista (Covarianza)
    // Puede recibir List<Integer>, List<Double>, List<Number>
    public static double calcularSuma(List<? extends Number> lista) {
        double suma = 0.0;
        for (Number num : lista) {
            suma += num.doubleValue(); // Es seguro leer como Number
        }
        // lista.add(3); // ERROR DE COMPILACIÓN: no se puede garantizar el tipo
        return suma;
    }

    // (ii) Uso de <? super T> para ESCRIBIR en la lista (Contravarianza)
    // Puede recibir List<Integer>, List<Number>, List<Object>
    public static void agregarEnteros(List<? super Integer> lista) {
        lista.add(10); // Es seguro añadir Integer
        lista.add(20);
        
        // Integer n = lista.get(0); // ERROR: Al leer, el compilador solo garantiza Object
    }
}
```</Object></String></Object></String></Object></String></Punto2D></T></Double></T></Integer></T></T></T></T></T></T,></Integer></String></T></K,></E></T>

```