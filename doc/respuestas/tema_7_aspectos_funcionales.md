# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un puntero a una función es una variable que almacena la dirección de memoria donde se encuentra el código ejecutable de una función concreta, en lugar de almacenar un valor de dato tradicional. Durante la compilación, las instrucciones que conforman una función se alojan en un bloque de memoria determinado; el puntero a función permite referenciar ese bloque y, consecuentemente, invocar a la función indirectamente en tiempo de ejecución.

El uso de punteros a funciones resulta fundamental en lenguajes como C, ya que es el mecanismo principal para pasar comportamientos como argumentos a otras funciones (comúnmente conocido como *callbacks*). Esto otorga una gran flexibilidad, permitiendo cambiar la función que se va a ejecutar dinámicamente según la lógica del programa, sin necesidad de emplear múltiples condicionales.

A continuación, se presenta un ejemplo en C. Se define una función que modifica una cadena en el sitio para convertirla a mayúsculas. Posteriormente, en la función `main`, se declara un puntero específico para firmas de funciones que reciben un `char*` y devuelven `void`, y se invoca la lógica a través de dicho puntero.

```c
#include <stdio.h>
#include <ctype.h>

// Definición de la función
void convertirMayusculas(char* cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
}

int main() {
    // Declaración del puntero a la función y asignación
    void (*aMayusculas)(char*) = convertirMayusculas;
    
    char texto[] = "hola mundo";
    
    // Invocación de la función a través del puntero
    aMayusculas(texto);
    
    printf("%s\n", texto); // Imprime: HOLA MUNDO
    return 0;
}

```

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una función lambda, también conocida comúnmente como función anónima, es un bloque de código que se define sin necesidad de asignarle un identificador o nombre formal en el momento de su creación. Estas funciones se utilizan típicamente para implementar lógicas breves que se van a usar una sola vez o que se van a pasar directamente como argumentos a otras funciones (funciones de orden superior).

La principal ventaja de las expresiones lambda es que permiten escribir código mucho más conciso, expresivo y cercano a la declaración del lugar donde realmente se va a utilizar la lógica. Al carecer de la sintaxis verbosa de una declaración de función tradicional (nombres, modificadores de acceso, tipos de retorno explícitos en algunos casos), se reduce considerablemente el código repetitivo (*boilerplate*), favoreciendo un estilo de programación más fluido y declarativo.

A continuación, se muestran las implementaciones solicitadas. En primer lugar, la versión en JavaScript utilizando las comúnmente denominadas "arrow functions" (funciones flecha).

```javascript
// Ejemplo en JavaScript
const aMayusculas = str => str.toUpperCase();

console.log(aMayusculas("hola mundo"));

```

En segundo lugar, se presenta la versión en Java. Debido a que Java posee un tipado estático riguroso, la expresión lambda debe ser asignada a una referencia cuyo tipo corresponda a una "interfaz funcional", en este caso `Function<String, String>`, la cual indica que toma un argumento de tipo `String` y devuelve otro de tipo `String`.

```java
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {
        // Ejemplo en Java
        Function<String, String> aMayusculas = str -> str.toUpperCase();
        
        System.out.println(aMayusculas.apply("hola mundo"));
    }
}

```

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El paradigma funcional es un estilo de programación declarativo que construye el software mediante la composición de funciones matemáticas puras. En contraste con el paradigma imperativo, donde el flujo de control se basa en cambiar el estado del programa a través de asignaciones sucesivas, la programación funcional trata de evitar la mutabilidad de los datos y los efectos secundarios. El objetivo principal es evaluar expresiones y aplicar transformaciones sobre flujos de datos de manera predecible.

A lenguajes como Java (a partir de su versión 8) o C++ se les clasifica como lenguajes multi-paradigma porque no obligan a adoptar un único enfoque arquitectónico. Originalmente, Java fue diseñado estrictamente bajo el paradigma orientado a objetos (donde todo gira en torno a clases, objetos y estado interno). Sin embargo, al incorporar herramientas como expresiones lambda, flujos de datos (`Streams`) e interfaces funcionales, el lenguaje permite a los desarrolladores mezclar lo mejor de ambos mundos: modelar el dominio con objetos y resolver algoritmos y transformaciones de datos utilizando técnicas del paradigma funcional.

El concepto de que las funciones son "ciudadanos de primera clase" (*first-class citizens*) significa que el lenguaje trata a las funciones exactamente igual que a cualquier otra variable o tipo de dato básico (como un entero o una cadena). Por lo tanto, una función puede ser almacenada en una variable, puede ser introducida en estructuras de datos, puede ser pasada como parámetro a otra función, y puede ser el valor de retorno explícito de una función u operación.

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

La sintaxis básica de una expresión lambda en Java se divide estructuralmente en tres componentes fundamentales: la lista de parámetros, el operador flecha y el cuerpo de la expresión. Su diseño busca minimizar la cantidad de código requerido para definir el comportamiento de una interfaz con un único método abstracto.

El primer elemento es la lista de parámetros, que se encierra entre paréntesis. Gracias al motor de inferencia de tipos del compilador de Java, en la gran mayoría de los casos no es necesario declarar explícitamente el tipo de dato de los parámetros. Además, si la función lambda recibe exactamente un único parámetro, los paréntesis pueden omitirse. Si no recibe ningún parámetro, se deben incluir paréntesis vacíos `()`.

El segundo elemento es el operador lambda, representado por una flecha `->`. Su única función es separar visual y sintácticamente los parámetros de entrada del bloque de código o expresión que los procesará.

Finalmente, el tercer elemento es el cuerpo. Si la lógica a ejecutar consta de una única instrucción, no se requieren llaves `{}` y el resultado de esa instrucción se devuelve automáticamente (sin necesidad de utilizar la palabra clave `return`). Si el cuerpo requiere múltiples líneas de código, es obligatorio envolverlas entre llaves y utilizar explícitamente la instrucción `return` si se espera que la lambda devuelva un valor.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Recibir una función como parámetro es el pilar de lo que se conoce como "funciones de orden superior". Esto permite que el comportamiento de un método sea inyectado desde el exterior, logrando un alto grado de desacoplamiento y reutilización de código. El método actúa como un orquestador, mientras que la función pasada define el detalle de la transformación.

En JavaScript, al tener un tipado dinámico, cualquier parámetro puede actuar como una función. Simplemente se recibe el parámetro que representa la función y se invoca utilizando los paréntesis convencionales.

```javascript
// Método de orden superior en JS
function transformar(texto, transformador) {
    return transformador(texto);
}

const aMayusculas = str => str.toUpperCase();
console.log(transformar("javascript", aMayusculas));

```

En Java, se debe definir claramente la interfaz funcional que representará la función esperada. En este caso, el método `transformar` se define requiriendo una instancia de `Function<String, String>`, y su invocación interna se realiza mediante el método abstracto que dicha interfaz define (en este caso, `.apply()`).

```java
import java.util.function.Function;

public class Main {
    // Método de orden superior en Java
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = str -> str.toUpperCase();
        System.out.println(transformar("java", aMayusculas));
    }
}

```

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

La principal virtud de las expresiones lambda no es almacenarlas en variables para usarlas posteriormente, sino definirlas *inline* justo en el momento y lugar donde son requeridas. Esto se conoce como pasar una lambda anónima y evita la proliferación de variables locales innecesarias que solo se emplean una vez a lo largo del flujo del programa.

Al pasar la lambda directamente en la invocación del método `transformar`, el código se vuelve sumamente conciso. A continuación se presenta cómo se realizaría esta operación en JavaScript, empleando las funciones nativas de manipulación de arreglos para invertir una cadena de texto.

```javascript
function transformar(texto, transformador) {
    return transformador(texto);
}

// Invocación pasando la lambda anónima directamente
const resultadoJS = transformar("javascript", s => s.split('').reverse().join(''));
console.log(resultadoJS); // tpircsavaj

```

Para el caso de Java, se realiza exactamente de la misma manera. Al recibir un `Function<String, String>`, el compilador de Java deduce automáticamente que la lambda `s -> ...` es la implementación requerida para ese argumento. Se emplea `StringBuilder` para realizar la inversión de la cadena de forma eficiente.

```java
import java.util.function.Function;

public class Main {
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        // Invocación pasando la lambda anónima directamente
        String resultadoJava = transformar("java", s -> new StringBuilder(s).reverse().toString());
        System.out.println(resultadoJava); // avaj
    }
}

```

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un cierre o "closure" es una característica clave en lenguajes que soportan funciones de primera clase, mediante la cual una función lambda no solo contiene el código a ejecutar, sino que también "captura" o "recuerda" el entorno (el contexto léxico) en el que fue definida. Esto significa que la función puede acceder a las variables locales del ámbito que la envuelve, incluso si esa función es ejecutada posteriormente en un ámbito o momento temporal completamente diferente.

En Java, existe una restricción importante respecto a las closures: las variables locales capturadas por una expresión lambda deben ser "finales" explícitamente, o al menos "efectivamente finales" (es decir, su valor no se modifica nunca tras la primera asignación). Esto se hace para evitar problemas de concurrencia y mantener la consistencia del estado capturado.

A continuación, se ilustra este concepto. Se define una variable local `sufijo` en el `main`. Luego, se define y pasa una lambda a la función `transformar`. La lambda utiliza `sufijo` dentro de su cuerpo. El bloque léxico de la lambda ha "capturado" el valor de la variable local del contexto exterior.

```java
import java.util.function.Function;

public class Main {
    public static String transformar(String texto, Function<String, String> transformador) {
        return transformador.apply(texto);
    }

    public static void main(String[] args) {
        // Variable local en el ámbito exterior
        String sufijo = " - Procesado";
        
        // La lambda "captura" la variable 'sufijo' (Closure)
        String resultado = transformar("Documento", s -> s + sufijo);
        
        System.out.println(resultado); // Imprime: Documento - Procesado
    }
}

```

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

La principal y más crítica diferencia radica precisamente en el concepto de "closure" o cierre léxico discutido en el punto anterior. Un puntero a función en C es simplemente una dirección de memoria estática que apunta a un bloque de instrucciones. No tiene conocimiento alguno del entorno donde fue referenciado; no puede acceder automáticamente a las variables locales de la función que creó dicho puntero. En C, para pasar "estado" junto con un comportamiento, se requiere pasar explícitamente estructuras de datos (como un `struct`) a través de un puntero genérico `void*` adicional.

Por el contrario, una función lambda en lenguajes modernos (como Java o JS) empaqueta dos cosas inseparables: el comportamiento (las instrucciones de código) y el estado contextual (el entorno léxico capturado). Detrás de escena, cuando se emplea una lambda que captura variables en Java, el compilador genera automáticamente una estructura u objeto temporal que guarda referencias o copias de esas variables locales, de manera que la lambda las tenga disponibles al momento de ejecutarse.

Otra diferencia significativa es el nivel de abstracción y el tipado. Los punteros a funciones en C operan muy a bajo nivel y pueden ser propensos a errores graves si las firmas no coinciden perfectamente. En lenguajes orientados a objetos con tipado fuerte, las lambdas se apoyan en el sistema de tipos y se resuelven contra interfaces funcionales específicas, brindando fuertes garantías en tiempo de compilación y permitiendo integración nativa con paradigmas como la genericidad.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

Devolver funciones como resultado de otra función es otra técnica característica del paradigma funcional (otra vertiente de las funciones de orden superior). Esto resulta ideal para construir "fábricas de funciones", donde se genera un comportamiento altamente especializado y preconfigurado que podrá ser ejecutado en el futuro múltiples veces.

En el siguiente ejemplo en Java, el método `crearDescuento` toma un porcentaje e instancia una función lambda. Esta lambda define la lógica de aplicar el descuento al `precio` de entrada. El método finaliza y retorna la lambda creada. Posteriormente, se generan dos políticas de descuento distintas y se aplican independientemente a un mismo precio base.

```java
import java.util.function.Function;

public class Main {
    // Función de orden superior que retorna otra función
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return precio -> precio - (precio * (porcentaje / 100.0));
    }

    public static void main(String[] args) {
        // Se crean dos funciones de descuento preconfiguradas
        Function<Double, Double> rebajasInvierno = crearDescuento(20.0);
        Function<Double, Double> liquidacion = crearDescuento(50.0);
        
        double precioBase = 100.0;
        
        System.out.println("Precio 20%: " + rebajasInvierno.apply(precioBase)); // 80.0
        System.out.println("Precio 50%: " + liquidacion.apply(precioBase));     // 50.0
    }
}

```

La "closure" juega aquí un papel mágico. Cuando la instrucción `rebajasInvierno.apply(100.0)` se ejecuta en el `main`, el método `crearDescuento` hace tiempo que finalizó su ejecución, y técnicamente sus parámetros locales (como `porcentaje`) habrían desaparecido de la pila. Sin embargo, la lambda devuelta actúa como una closure y ha "capturado" y guardado internamente el valor `20.0`. Por tanto, sobrevive al ciclo de vida del método que la originó, permitiendo que la lambda aplique el porcentaje correcto independientemente de cuándo y dónde sea invocada posteriormente.

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

En Java, dado que no existen "tipos de función" puros como los que existen en C o en lenguajes funcionales estrictos, el mecanismo elegido para integrar las lambdas dentro de su estricto sistema de tipado estático orientado a objetos son las **interfaces funcionales**. Una interfaz funcional es simplemente una interfaz convencional de Java que se utiliza como el "tipo de destino" (target type) al que se asigna o se infiere una expresión lambda.

El requisito absoluto e inamovible para que una interfaz sea considerada funcional es que debe contener **exactamente un único método abstracto** (también conocido como modelo SAM: *Single Abstract Method*). Este único método define la firma (parámetros y tipo de retorno) que obligatoriamente deberá cumplir cualquier función lambda que pretenda implementarla. Si la interfaz definiera dos métodos abstractos, el compilador sería incapaz de deducir a cuál de los dos intenta dar cuerpo la expresión lambda.

Para aportar claridad semántica y seguridad en tiempo de compilación, Java introdujo la anotación opcional `@FunctionalInterface`. Aunque no es estrictamente obligatoria (cualquier interfaz con un solo método abstracto es tratada automáticamente como funcional), colocar esta anotación instruye al compilador para que emita un error si, por accidente, algún desarrollador añade en el futuro un segundo método abstracto, previniendo así la ruptura del código que dependa de lambdas. (Cabe destacar que los métodos por defecto `default` o los métodos estáticos dentro de la interfaz no cuentan para esta regla).

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

La creación manual de interfaces funcionales es muy sencilla y resulta útil cuando se desea dotar al código de un significado fuertemente asociado a la lógica de negocio, haciendo que las firmas de los métodos sean más descriptivas que el uso de interfaces genéricas predefinidas.

En este caso, se define la interfaz `Transformador`. Se incluye la anotación de buenas prácticas y se declara un único método abstracto llamado `transformar`, el cual espera recibir una cadena de texto y devolver el resultado procesado en otra cadena de texto.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String in);
}

```

A partir de esta definición, en lugar de utilizar `Function<String, String>` en los ejemplos anteriores, se puede requerir directamente un objeto de tipo `Transformador`. La asignación de la lambda sería idéntica: `Transformador aMayusculas = s -> s.toUpperCase();`, y la invocación del comportamiento se haría llamando al método definido: `aMayusculas.transformar("texto");`.

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Para evitar tener que crear una interfaz `TransformadorString`, otra `TransformadorDoubleAEntero`, etc., se puede combinar el poder de la programación genérica con las interfaces funcionales. Al añadir parámetros de tipo a la interfaz, el método abstracto puede definirse en función de dichos tipos genéricos, convirtiendo la interfaz en una plantilla reutilizable para cualquier combinación de tipos de entrada y salida.

Se definen dos parámetros genéricos convencionales: `<T>` para el tipo de entrada ("Type") y `<R>` para el tipo de retorno ("Return"). El método abstracto reflejará esta firma genérica. A continuación se define la interfaz y un ejemplo práctico instanciando un transformador concreto que convierte números reales en números enteros.

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T in);
}

public class Main {
    public static void main(String[] args) {
        // Se instancian los tipos genéricos: T=Double, R=Integer
        Transformador<Double, Integer> redondear = d -> (int) Math.round(d);
        
        Integer resultado = redondear.transformar(4.7);
        System.out.println("Valor redondeado: " + resultado); // Imprime: 5
    }
}

```

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

En efecto, interfaces como la genérica creada anteriormente son tan fundamentales en el día a día que la API estándar de Java provee un paquete completo (`java.util.function`) que contiene docenas de interfaces funcionales predefinidas. Su propósito es estandarizar y cubrir casi cualquier necesidad de firmas de métodos que un desarrollador pueda requerir, evitando la duplicación de definiciones a lo largo de las bibliotecas.

Las interfaces funcionales predefinidas más importantes se agrupan en cuatro categorías principales basadas en su comportamiento (entradas y salidas):

1. **Function `<T, R>`:** Recibe un argumento de tipo T y devuelve un resultado de tipo R. Representa una transformación de datos (ej. `String -> Integer`). Método abstracto: `apply(T t)`.
2. **Consumer `<T>`:** Recibe un argumento de tipo T, realiza una operación sobre él, pero no devuelve ningún resultado (retorna `void`). Representa efectos secundarios (ej. imprimir por pantalla o guardar en base de datos). Método abstracto: `accept(T t)`.
3. **Supplier `<T>`:** No recibe ningún argumento de entrada, pero devuelve un resultado de tipo T. Representa fábricas de objetos, generación de datos o lecturas pospuestas. Método abstracto: `get()`.
4. **Predicate `<T>`:** Recibe un argumento de tipo T y devuelve un valor primitivo `boolean`. Se utiliza masivamente para comprobaciones lógicas y filtros de datos. Método abstracto: `test(T t)`.

Adicionalmente, existen variaciones especializadas como `UnaryOperator<T>` (un `Function` donde entrada y salida son del mismo tipo), `BiFunction<T, R U,>` (recibe dos argumentos), y versiones optimizadas para evitar el empaquetado/desempaquetado (boxing/unboxing) de tipos primitivos (como `IntFunction`, `DoubleConsumer`, etc.).

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método `forEach` incluido en la interfaz `Iterable` (de la cual hereda `List`) es una forma declarativa de recorrer los elementos de una colección. En lugar de escribir explícitamente la lógica de iteración con índices y bucles `for` (control de flujo imperativo), se le delega a la propia colección la tarea de iterar, indicándole únicamente "qué hacer" con cada elemento iterado a través de una expresión lambda.

Desde el punto de vista del tipado, el método `forEach` espera recibir una instancia de la interfaz funcional `Consumer`, lo que significa que la lambda debe recibir un elemento y devolver `void`. A continuación se muestra cómo emplear esta técnica funcional para realizar un filtrado condicional básico.

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-3, 5, -1, 8, 0, 12);

        // Iteración declarativa: "Para cada número, haz esto..."
        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("Encontrado número positivo: " + n);
            }
        });
    }
}

```

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

El uso de bounds (límites) como `<? super T>` se apoya en el principio de contravarianza para ofrecer máxima flexibilidad. Si se usara un rígido `Consumer<T>`, una lista de `Integer` obligaría a pasarle exclusivamente un consumidor de enteros. Al emplear `Consumer<? super T>`, la API acepta un consumidor de enteros, pero también permite pasarle, por ejemplo, un consumidor de `Number` o incluso un consumidor de `Object`. Esto tiene sentido lógico: si se tiene una función general capaz de imprimir un `Object`, esa misma función es perfectamente capaz y segura de procesar los `Integer` extraídos de la lista.

El mnemotécnico **PECS** (*Producer Extends, Consumer Super*) es la regla de oro en el uso de genéricos introducida por Joshua Bloch. Establece cómo usar wildcards para maximizar la compatibilidad:

* **Producer Extends:** Si una estructura te va a proveer o suministrar datos de tipo `T` (la estructura produce para ti), se debe usar `<? extends T>`.
* **Consumer Super:** Si vas a insertar datos o cederle datos a una estructura/función para que ella opere con ellos (la estructura consume lo tuyo), se debe usar `<? super T>`.

Aplicando PECS al método `transformar` que definimos en preguntas anteriores (`transformar(T texto, Function<T, R> transformador)`), la forma más flexible y correcta de la firma sería `Function<? super T, ? extends R>`. Explicado bajo PECS: la función transformadora actúa como **Consumidora** respecto a la entrada `T` (por lo que acepta operar con `T` o cualquier superclase superior a `T`) y actúa como **Productora** respecto a la salida `R` (por lo que garantiza generar un dato que, en el peor de los casos, es una subclase y encajará con éxito en `R`).

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

Las referencias a métodos son una sintaxis abreviada, conocida como azúcar sintáctico, que se emplea cuando una expresión lambda lo único que hace internamente es llamar directamente a un método ya existente sin alterar ni procesar en absoluto los argumentos. Al capturar la referencia del método, se evita definir parámetros y flechas, logrando un código aún más legible.

En JavaScript, las funciones son objetos, por lo que es trivial asignar un método a una variable. Sin embargo, surge el problema de la pérdida del contexto léxico (`this`). Para garantizar que la referencia extraída siga apuntando a los datos del objeto original al ser invocada aisladamente, se debe emplear explícitamente el mecanismo de ligado o *binding*.

```javascript
class Persona {
    constructor(nombre) { this.nombre = nombre; }
    saludar() { console.log("Hola, soy " + this.nombre); }
}

const personaJS = new Persona("Ana");
// Se obtiene la referencia, ligándola al contexto original para no perder 'this'
const referenciaSaludarJS = personaJS.saludar.bind(personaJS);

referenciaSaludarJS(); // Imprime: Hola, soy Ana

```

En Java, se emplea el operador de doble dos puntos `::` para obtener la referencia al método. Al asignar esta referencia a una interfaz funcional compatible (como `Runnable`, que no recibe parámetros y retorna `void`), el compilador maneja automáticamente el contexto de la instancia sobre la que operará dicho método.

```java
class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    public void saludar() { System.out.println("Hola, soy " + this.nombre); }
}

public class Main {
    public static void main(String[] args) {
        Persona personaJava = new Persona("Ana");
        
        // Se obtiene la referencia al método de esa instancia en particular
        Runnable referenciaSaludarJava = personaJava::saludar;
        
        referenciaSaludarJava.run(); // Imprime: Hola, soy Ana
    }
}

```

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

En Java, el operador `::` soporta cuatro variaciones o tipos distintos de referencias a métodos, adaptándose al contexto y a las necesidades específicas de la firma de la interfaz funcional objetivo.

1. **Referencia a un método estático:** Se invoca a través del nombre de la clase. Útil para funciones utilitarias.
* *Sintaxis:* `Clase::metodoEstatico`
* *Ejemplo:* `Function<String, Integer> parseador = Integer::parseInt;` (equivale a `s -> Integer.parseInt(s)`).


2. **Referencia a un constructor:** Se emplea la palabra reservada `new`. Su objetivo es actuar como fábricas de objetos, enlazándose a interfaces como `Supplier` o funciones personalizadas de instanciación.
* *Sintaxis:* `Clase::new`
* *Ejemplo:* `Supplier<List<String>> creadorLista = ArrayList::new;` (equivale a `() -> new ArrayList<>()`).


3. **Referencia a un método de instancia de un objeto concreto:** (Es el tipo visto en la pregunta 16). Se llama al método sobre una variable/objeto ya instanciado.
* *Sintaxis:* `instanciaConcreta::metodo`
* *Ejemplo:* `Consumer<String> impresor = System.out::println;` (equivale a `s -> System.out.println(s)`).


4. **Referencia a un método de instancia de un objeto arbitrario de un tipo particular:** Este es el caso más complejo. La instancia sobre la que se llamará el método no se conoce en el momento de la asignación, sino que será pasada como el primer argumento en el momento de la invocación.
* *Sintaxis:* `Clase::metodoDeInstancia`
* *Ejemplo:* `Function<String, Integer> longitud = String::length;` (equivale a `s -> s.length()`).



## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

Para este requerimiento, se busca ordenar la lista aplicando un criterio principal y, en caso de empate, un criterio secundario de desempate. Esto demanda crear implementaciones específicas de la interfaz funcional `Comparator<T>`.

La primera versión utiliza una expresión lambda explícita, creando el comparador manualmente y programando toda la lógica algorítmica y condicional de la comparación.

```java
import java.util.*;

// (Supongamos la clase Persona con getters: getNombre() y getEdad())

public class Main {
    public static void ordenarManual(List<Persona> personas) {
        // Versión 1: Lógica manual en el cuerpo de la lambda
        Collections.sort(personas, (p1, p2) -> {
            int comparacionEdad = Integer.compare(p1.getEdad(), p2.getEdad());
            if (comparacionEdad != 0) {
                return comparacionEdad;
            } else {
                return p1.getNombre().compareTo(p2.getNombre());
            }
        });
    }
}

```

La segunda versión demuestra el tremendo poder expresivo de combinar las interfaces funcionales predefinidas de Java 8, los métodos estáticos de interfaz y las referencias a métodos. Se utiliza la clase constructora utilitaria `Comparator`, delegando toda la complejidad imperativa en métodos encadenados, logrando que el código lea casi como lenguaje natural.

```java
import java.util.*;

public class Main {
    public static void ordenarElegante(List<Persona> personas) {
        // Versión 2: Empleando métodos estáticos y composición de Comparator
        Collections.sort(personas, 
            Comparator.comparingInt(Persona::getEdad)
                      .thenComparing(Persona::getNombre)
        );
    }
}
```</T></String,></String></List<String></String,></T,></T></T></T,></T></T></T></T></T,></T,></R></T></String,></Double,></String,></String,></String,></String,>

```