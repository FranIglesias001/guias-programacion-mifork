# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

En lenguajes estructurados como C, el control de errores se basa fundamentalmente en la comprobación manual. Puesto que no existe un mecanismo automático para interrumpir el flujo y saltar a un bloque de gestión de errores, la comunicación de que algo ha fallado debe hacerse mediante los valores de retorno de las funciones o a través de los parámetros que se pasan por referencia (punteros).

La primera opción de diseño consiste en reservar un valor de retorno específico que indique el error. En el caso de una raíz cuadrada, dado que el resultado siempre debe ser un número positivo (o cero), se puede utilizar un valor negativo (como `-1.0`) para señalar al código llamador que se ha producido una infracción matemática. Es responsabilidad del llamador comprobar si el resultado devuelto es ese valor reservado.

```c
#include <stdio.h>
#include <math.h>

// Opción 1: Devolver un valor especial indicando error (-1.0)
double raiz(double radicando) {
    if (radicando < 0) {
        return -1.0; 
    }
    return sqrt(radicando);
}

int main() {
    double resultado = raiz(-4.0);
    if (resultado == -1.0) {
        printf("Error: No se puede calcular la raiz de un numero negativo.\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }
    return 0;
}

```

La segunda opción consiste en separar conceptualmente el estado de la operación del resultado matemático. Para ello, la función devuelve un código de estado (típicamente un número entero donde `0` significa éxito y un valor distinto de cero indica error), mientras que el resultado real de la operación matemática se "devuelve" modificando una variable externa que se ha pasado por referencia mediante un puntero.

```c
#include <stdio.h>
#include <math.h>

// Opción 2: Devolver código de error y pasar el resultado por referencia
int raiz_segura(double radicando, double *resultado) {
    if (radicando < 0) {
        return 1; // 1 indica código de error
    }
    *resultado = sqrt(radicando);
    return 0; // 0 indica éxito
}

int main() {
    double valor;
    int codigo_error = raiz_segura(-4.0, &valor);
    
    if (codigo_error != 0) {
        printf("Error: Operacion invalida.\n");
    } else {
        printf("Resultado: %f\n", valor);
    }
    return 0;
}

```

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una excepción es un evento anómalo que ocurre durante la ejecución de un programa e interrumpe el flujo normal de las instrucciones. A diferencia de C, donde los errores se representan con simples números o valores nulos, en lenguajes modernos orientados a objetos una excepción es una entidad propia (un objeto) que encapsula información detallada sobre el problema que acaba de suceder, como su tipo, un mensaje descriptivo y la línea exacta de código donde se originó.

El objetivo principal al usar excepciones es separar claramente el código que implementa la lógica principal de la aplicación ("el camino feliz") del código que gestiona los errores. Cuando se implementa una función, lanzar una excepción permite detener inmediatamente la operación en curso ante un estado inválido, sin obligar al programador a inventar códigos de retorno mágicos.

Por otro lado, cuando se llama a una función, el mecanismo de excepciones obliga, o al menos permite de forma muy limpia, que el desarrollador prevea qué hacer si algo falla. Esto evita que los errores pasen desapercibidos (como ocurre a menudo en C si se olvida comprobar un `if (error != 0)`) y proporciona un canal seguro para delegar la responsabilidad del error a niveles superiores de la aplicación.

Prof:
Excepción -> srge en situaciones atípicas.
    - Cuando implementamos -> Nos permite indicar más claramente el error.
    - Cuando llamamos -> me facilta separar la lógica normal de la de reacción o manejo de la situación errónea.
## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java, el paradigma cambia hacia la orientación a objetos y el uso de excepciones integradas en el lenguaje. La lógica de cálculo se encapsula en una clase, y si se detecta una precondición no válida (como un número negativo), se interrumpe la ejecución instanciando y lanzando un objeto de tipo excepción con la palabra reservada `throw`.

Desde el exterior, en el método `main`, la llamada al método de la calculadora se envuelve en un bloque de control `try-catch`. Este bloque permite aislar el intento de ejecutar la operación, y en caso de que la calculadora "grite" (lance el error), el bloque `catch` actúa como un salvavidas que atrapa el objeto excepción, evitando que el programa termine abruptamente y permitiendo gestionar la respuesta.

```java
class Calculadora {
    // Método que evalúa la precondición y lanza la excepción si no se cumple
    public double raiz(double radicando) {
        if (radicando < 0) {
            throw new IllegalArgumentException("No se puede calcular la raiz de un numero negativo.");
        }
        return Math.sqrt(radicando);
    }
}

public class Main {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        
        try {
            // Intentamos ejecutar el código que podría fallar
            double resultado = calc.raiz(-4.0);
            System.out.println("El resultado es: " + resultado);
            
        } catch (IllegalArgumentException e) {
            // Se controla el error desde fuera usando la información del objeto excepción
            System.out.println("Error detectado: " + e.getMessage());
        }
        
        System.out.println("El programa continua su ejecucion de forma normal...");
    }
}

```

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

**Lanzar** (o *throw*) una excepción es la acción que toma una función para advertir que se ha topado con un problema insalvable, creando un objeto que representa ese error y arrojándolo fuera de su flujo normal de ejecución. Por el contrario, **controlar** o **capturar** (o *catch*) es la acción que toma el código llamador para interceptar ese objeto en el aire antes de que colapse el programa, con el fin de tomar medidas correctivas o informar al usuario.

La **propagación** ocurre cuando una función lanza una excepción pero no la captura internamente. En este caso, la excepción abandona inmediatamente esa función y "cae" hacia la función que la llamó. Si esa función llamadora tampoco tiene un bloque `catch`, la excepción sigue cayendo hacia la siguiente función en la pila de llamadas (el *call stack*), y así sucesivamente hasta que se encuentre un controlador o el programa "muera" a nivel del sistema operativo.
Durante esta propagación, las funciones de la pila que son atravesadas por la excepción se cancelan abruptamente. Todas las variables locales de esas funciones se destruyen y su ejecución se da por terminada prematuramente. Es de vital importancia entender que **no hay forma de reanudar** una función en el punto exacto donde dejó de ejecutarse por culpa de una excepción no controlada; ese flujo se ha perdido para siempre.

En el ejemplo de la raíz cuadrada en Java, cuando el método `raiz` recibe un `-4.0`, ejecuta el `throw` y su ejecución termina en esa misma línea (jamás ejecuta el `Math.sqrt`). La excepción se propaga hacia atrás, aterrizando en el método `main`. Puesto que `main` ha provisto un bloque `catch`, detiene la propagación de la excepción, ejecuta el código de recuperación (el mensaje por pantalla) y, a partir de ahí, el método `main` sí puede continuar ejecutando sus siguientes líneas, pero el método `raiz` original no se reanuda.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La principal ventaja es la limpieza y claridad del código fuente. En C, si una función de bajo nivel (por ejemplo, una que lee un bloque de disco) encuentra un error, debe devolver un código de error a la función de nivel medio que la llamó, la cual a su vez debe comprobar ese código mediante un `if` y devolver otro código de error a la función de alto nivel. Esto genera un efecto dominó de comprobaciones manuales que engorda el código y ensucia la lógica de negocio.

Con la propagación natural de excepciones, las funciones intermedias no necesitan enterarse ni participar activamente en la ruta del error. Si la función de bajo nivel lanza la excepción, esta atravesará de forma transparente y automática todas las capas intermedias, llegando directamente al módulo de alto nivel diseñado para interactuar con el usuario y gestionar los fallos.

Esto promueve una fuerte cohesión y evita el clásico problema de C en el que, si un programador olvida poner el `if` de comprobación en una de las funciones intermedias, el error se traga silenciosamente provocando fallos impredecibles más adelante. En los lenguajes con excepciones, el error es ruidoso por defecto: si no se atiende activamente, el programa falla de manera controlada y evidente, garantizando que ninguna anomalía crítica pase desapercibida.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

Sí, en la programación orientada a objetos (POO), las excepciones son instancias de clases especializadas. En lugar de ser un simple número entero o una cadena de texto plana, se trata de objetos completos que poseen atributos (estado) y métodos (comportamiento), derivando todos ellos de una jerarquía común proporcionada por el lenguaje.

Esto aporta todas las ventajas inherentes a la encapsulación. Un objeto excepción actúa como una cápsula o "caja negra" que viaja desde el punto del error hasta el punto de captura, transportando consigo datos complejos sobre el incidente. El creador de la excepción puede encapsular dentro de este objeto valores específicos (por ejemplo, el número erróneo que causó el fallo, el nombre del archivo corrupto o la marca de tiempo), y el receptor solo necesita invocar métodos públicos (como `getMessage()` o `getStackTrace()`) para extraer lo que necesite, sin conocer los detalles de cómo se recolectó esa información.

Al ser simples clases, es completamente posible y muy recomendable crear excepciones personalizadas. Basta con definir una nueva clase que herede de la clase de excepción base del lenguaje. De este modo, un dominio específico puede tener sus propios tipos de errores (por ejemplo, `SaldoInsuficienteException` en una aplicación bancaria), lo que permite que los bloques de captura identifiquen el error exacto por su tipo de clase, en lugar de tener que adivinar el problema analizando un código numérico genérico.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta
Prof:
a. Un mensaje (getMessage()).
b. La traza de la pila (getSrackTrace, printStackTrace).
c. Opcionalmente, la "Causa", es otra excepción que es la verdadera causa

En el ejemplo de C, la única información transmitida hacia el manejador es un número entero (`1` o `-1.0`). Para entender qué significa ese número, el programador debe consultar documentación externa o variables globales, y carece por completo de contexto sobre en qué línea exacta del código ocurrió el fallo, especialmente en sistemas complejos.

En contraste, un objeto excepción en Java viaja al manejador cargado de información vital. En primer lugar, lleva el **tipo de la excepción**, que gracias a la jerarquía de clases ya indica la categoría del problema (por ejemplo, `IllegalArgumentException` dice claramente que el fallo radica en los parámetros pasados). En segundo lugar, contiene un **mensaje de texto descriptivo**, provisto por el programador en el momento del lanzamiento, que aclara de forma legible para humanos la naturaleza precisa del error.

Por último, y quizás lo más importante para la depuración, el objeto excepción transporta la **traza de la pila** (*stack trace*). Se trata de una radiografía exacta del estado del programa en el milisegundo en que ocurrió el error, listando en orden todas las funciones que estaban activas, el nombre de los archivos fuente involucrados y, crucialmente, la línea exacta de código donde se originó el fallo y la ruta que siguió hasta ser capturado.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

Sí, es completamente posible encadenar varios bloques `catch` sucesivos después de un único bloque `try`. Esto se utiliza habitualmente cuando el código protegido dentro del `try` es susceptible de disparar distintos tipos de errores (por ejemplo, podría fallar por formato de número inválido o por intentar leer de un fichero inexistente), permitiendo definir un tratamiento o un mensaje específico para cada situación concreta.

Sin embargo, de todos los bloques `catch` definidos, **solo se ejecuta como máximo uno**. Cuando se lanza la excepción en el `try`, la máquina virtual de Java inspecciona los `catch` de arriba hacia abajo. En el momento en que encuentra el primer bloque cuyo tipo de excepción coincida (o sea una clase padre) con el de la excepción lanzada, entra en ese bloque, ejecuta su código, e ignora automáticamente todos los bloques `catch` restantes, saltando al final de la estructura de control.

Prof:
- Puede haber t de un catch
- Se ejecuta UNO:
    - Se va comprobando por orden de declaración del catch, el primero que encaje es el que se ejecuta.
    => NOTA: se deben ordenar de más específica a más general (de arriba a abajo).

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

Para asegurar que ciertas operaciones críticas, como el cierre de archivos, conexiones a bases de datos o liberación de bloqueos, se realicen pase lo que pase, los lenguajes como Java disponen de la cláusula `finally`. El bloque `finally` se coloca siempre al final de una estructura `try`, y su característica fundamental es que se garantiza su ejecución absoluta, independientemente de si el bloque `try` finalizó con éxito o de si se interrumpió lanzando una excepción.

De este modo, la propagación de la excepción se pone momentáneamente "en pausa". Si se produce un error, se ejecuta el `catch` (si existe), a continuación se ejecuta obligatoriamente el `finally`, y solo después de esto la excepción continúa su camino de propagación si no fue suprimida.

```java
// Ejemplo 1: try-catch-finally (capturando la excepción)
public void procesarArchivoConCatch() {
    try {
        System.out.println("Abriendo archivo...");
        // Simulamos un error
        throw new RuntimeException("Error de lectura");
    } catch (RuntimeException e) {
        System.out.println("Se atrapo el error: " + e.getMessage());
    } finally {
        System.out.println("Cerrando archivo... Esto se ejecuta siempre.");
    }
}

// Ejemplo 2: try-finally (sin catch, la excepción se propaga hacia arriba)
public void procesarArchivoSinCatch() {
    try {
        System.out.println("Abriendo conexion...");
        // Simulamos un error
        throw new RuntimeException("Error de red");
    } finally {
        System.out.println("Cerrando conexion... Esto se ejecuta, y luego el error sigue propagándose.");
    }
}

```

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

Sí, un bloque `finally` puede acompañar a un bloque `try` sin que exista ningún bloque `catch` de por medio. Esta estructura es sumamente útil cuando la función actual no tiene la responsabilidad (ni los conocimientos) para manejar el error, por lo que desea que la excepción se propague libremente hacia las capas superiores, pero aún así necesita asegurar la liberación de los recursos locales que haya abierto antes de ceder el control.

La garantía de ejecución del bloque `finally` es extremadamente estricta. Se ejecuta siempre, tanto si el bloque `try` llega a su fin de forma natural sin alteraciones, como si se interrumpe de forma súbita por el lanzamiento de una excepción.

Esta garantía se mantiene firme incluso frente a instrucciones de salto como `return`, `break` o `continue`. Si dentro del bloque `try` se alcanza una instrucción `return`, el entorno de ejecución evalúa el valor a devolver, pero antes de abandonar efectivamente la función y ceder la ejecución al llamador, interrumpe el proceso para ejecutar todo el contenido del bloque `finally`. Solo cuando el `finally` termina, se hace efectivo el retorno a la función superior.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

En Java, el sistema divide las excepciones en dos grandes categorías estructurales. Las excepciones **controladas** (o *checked exceptions*) son aquellas que el compilador de Java obliga estrictamente a prever. Si un método puede emitir una de estas, el código está obligado a envolver la llamada en un `try-catch` o a declarar en su firma que también la propagará. Las excepciones **no controladas** (o *unchecked exceptions*) son eximidas de esta regla; el compilador no obliga a atraparlas ni a declararlas, delegando en el sentido común del programador si debe manejarlas o dejarlas caer hasta terminar el programa.
El papel divisor lo juega la clase `RuntimeException`. Cualquier clase de excepción que herede directa o indirectamente de `RuntimeException` es considerada por el compilador como "no controlada". Todas las demás excepciones que heredan de la clase base genérica `Exception` (pero no de `RuntimeException`) son forzosamente "controladas". Por ejemplo, un tipo típico de excepción controlada es `IOException` (error de entrada/salida), y de no controlada es `NullPointerException` (intento de usar un objeto nulo).

**Situaciones donde se suele preferir una excepción controlada (Checked):**

* Intentar leer de un archivo en disco que ha sido movido o borrado por el usuario externo.
* Intentar conectar a una base de datos remota cuando hay una caída de la red de internet.
* Conectar a un servicio web que solicita contraseña y las credenciales proporcionadas han expirado.
* *(General)*: Situaciones donde el error proviene del entorno externo, es previsible, y se espera que el programa se recupere razonablemente del fallo.

**Situaciones donde se suele preferir una excepción no controlada (Unchecked - RuntimeException):**

* Intentar acceder a la posición 10 de un array que solo tiene 5 elementos.
* Pasar un número negativo a una función de cálculo matemático que estrictamente exigía un positivo.
* Intentar invocar un método sobre una variable que apunta a `null` por un olvido en la instanciación.
* *(General)*: Situaciones que reflejan fallos lógicos o *bugs* directos en el código escritos por el programador, donde la recuperación no tiene sentido y el programa debe ser corregido en desarrollo.

Prof:
- Excepciones "controladas" => Típicamente errores por casusas externas y que sempre pueden llegar a ocurrir, ej: errores de E/S. (Obliga a poner try-catch/throws).

- Excepciones "no controladas" => Típicamente errores de programación que, una vez solventados no vuelven a ocurrir. (No estamos obligados a poner try-catch/throws).

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

La palabra reservada `throws` se utiliza en la definición de la firma de un método (justo antes de las llaves de apertura) para declarar oficialmente qué excepciones de tipo controladas puede emitir dicho método hacia el exterior durante su ejecución. Actúa como un contrato o una advertencia en la documentación del método, indicando a cualquiera que pretenda llamarlo que ciertas contingencias peligrosas podrían ocurrir.

Se considera la alternativa directa a la captura mediante `try-catch` porque es el mecanismo mediante el cual un programador se libera de la obligación de tratar la excepción en ese mismo momento. Cuando el compilador exige tratar una excepción controlada, la opción de colocar `throws` significa elegir la propagación consciente; es decir, el método actual admite no saber cómo recuperarse del problema, y pasa legalmente la pelota de la responsabilidad de manejar el error a la función superior en la jerarquía.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

Para ilustrar este comportamiento, se muestra un método que simula la apertura de un fichero. Al utilizar la cláusula `throws IOException` en la cabecera, el método rechaza explícitamente atrapar el fallo con un `catch`, delegando el control a quien lo llame, pero al mismo tiempo asegura la integridad de los recursos usando un `try-finally` para cerrar lo que estuviera abierto.

```java
import java.io.IOException;

public class GestorArchivos {
    
    // El método declara en su firma que puede propagar una IOException hacia arriba
    public void procesarFichero(String ruta) throws IOException {
        System.out.println("Intentando abrir recursos para el fichero en: " + ruta);
        
        try {
            // Se simula un fallo de lectura/escritura (fichero no existe)
            // Aquí ocurriría el error real y la función saltaría
            throw new IOException("El fichero no ha sido encontrado en el disco.");
            
        } finally {
            // Aunque la IOException se haya lanzado hacia el llamador,
            // este bloque garantizará que cerremos cualquier recurso intermedio.
            System.out.println("Liberando el canal de disco y cerrando el fichero pase lo que pase.");
        }
    }
}

```

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Sí, la sintaxis del lenguaje Java permite perfectamente incluir excepciones no controladas (aquellas derivadas de `RuntimeException`) en la cláusula `throws` de la firma de un método. No obstante, que se incluyan no cambia las reglas fundamentales del compilador respecto a este tipo de excepciones.

Incluso si un método declara `throws IllegalArgumentException`, el método llamador **no está obligado** a colocar un bloque `try-catch`. El compilador de Java lo tratará con la misma flexibilidad de siempre y compilará sin errores, permitiendo que la excepción suba por la pila de llamadas hasta romper el programa si nadie interviene voluntariamente.

El sentido principal de hacer esto no es técnico, sino puramente documental. Incluir una excepción no controlada en el `throws` es una forma explícita e integrada en el código de advertir a otros programadores de la API (y de informar a las herramientas automáticas de generación de documentación como Javadoc) sobre las precondiciones estrictas de la función y los posibles fallos de lógica que pueden provocar una ruptura abrupta del flujo.

Prof:
- por poder se puede RunTimeException
- El compilador no obliga a try-catch/throws en el código llamador.
- Por propósitos de documentación
- No es lo habitual


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

La recomendación general en el diseño de software establece que se deben utilizar excepciones **controladas** cuando el problema escapa completamente al control del programador y proviene del entorno (redes intermitentes, permisos de archivos, bases de datos caídas), y existe una expectativa razonable de que la aplicación cliente pueda y deba tomar medidas para recuperarse (como reintentar la conexión o pedir al usuario otra ruta de archivo). Por contra, se deben utilizar excepciones **no controladas** cuando el error es consecuencia de un fallo de lógica en la programación, como enviar parámetros inválidos o estados corruptos, escenarios donde "recuperarse" carece de sentido sin antes arreglar el código fuente.

No todos los lenguajes de programación poseen esta dicotomía. La división entre excepciones controladas y no controladas es una característica bastante específica de Java. Otros lenguajes modernos o ampliamente usados, como C#, C++, Python, Ruby o JavaScript, no hacen esta distinción formal a nivel de compilador.

En aquellos lenguajes que no disponen de la doble opción, la alternativa que prevalece de manera unánime es el uso de **excepciones no controladas**. En la evolución del diseño de lenguajes, se ha llegado a la conclusión mayoritaria de que obligar explícitamente a controlar excepciones (como hace Java con las controladas) genera código muy acoplado e inunda los programas de bloques vacíos donde los desarrolladores atrapan y silencian el error para calmar al compilador. Por ello, la tendencia general es permitir que cualquier excepción fluya libremente, asumiendo que el programador sabrá controlarla en la capa arquitectónica más adecuada.

Prof:
- No hay ambas opciones en todos los lenguajes.
- La más típica es la de "no controladas".

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Sí, lanzar nuevas excepciones dentro de un bloque `catch` es una práctica muy habitual y justificada. A menudo, una capa intermedia de la aplicación atrapa una excepción de nivel muy bajo (por ejemplo, un error concreto de sintaxis de base de datos SQL) y la "traduce", lanzando una nueva excepción de negocio más genérica hacia la interfaz de usuario, ocultando así los detalles técnicos internos.

De igual manera, es totalmente posible volver a lanzar (relanzar) la misma excepción que se acaba de capturar utilizando simplemente `throw e;` (donde `e` es la variable del `catch`). Esta técnica cobra sentido cuando una función necesita intervenir temporalmente en el flujo del error para realizar una acción local (como guardar un registro en un archivo log informando del fallo), pero reconoce que no es la responsable final de solucionar el problema, permitiendo que la excepción continúe su viaje intacta hacia niveles superiores.

```java
// Ejemplo 1: Lanzar una nueva excepción dentro del catch (Traducción de excepción)
public void consultarUsuario() {
    try {
        // Simulación de error de bajo nivel
        throw new SQLException("Tabla 'users' no encontrada en la BBDD.");
    } catch (SQLException e) {
        // Se lanza un error de alto nivel comprensible por el frontend
        throw new UsuarioNoEncontradoException("Hubo un problema recuperando la cuenta del usuario.");
    }
}

// Ejemplo 2: Relanzar la misma excepción
public void procesarTransaccion() {
    try {
        // Simulación de error en la lógica
        throw new IllegalArgumentException("Monto de transaccion negativo.");
    } catch (IllegalArgumentException e) {
        // Se interviene para registrar el error
        System.out.println("LOG INTERNO: Ocurrió un intento de fraude o error de cantidad.");
        // Se relanza la misma excepción para que la capa superior rechace la operación
        throw e; 
    }
}

```
Prof:
- Sí, tiene sentido.
- Sí se puede re-lanzar el mismo objeto excepción, tras, por ejemplo, haber ejecutado algo en catch.


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

El concepto de "causa" (conocido como *Exception Chaining* o encadenamiento de excepciones) consiste en introducir la excepción original de bajo nivel que originó el fallo dentro de la nueva excepción de alto nivel que se va a lanzar. Al hacer esto, la excepción original se convierte en un atributo persistente dentro de la nueva, permitiendo "traducir" el error para una capa superior, pero sin destruir las pruebas forenses del problema real, como la línea exacta de código donde se generó el fallo primitivo.

En Java, esto se logra pasando la excepción atrapada como parámetro al constructor de la nueva excepción. Posteriormente, cuando el programa falla y el sistema vuelca la traza por la consola o un archivo de registro, la causa sí resulta completamente visible. La salida suele mostrar primero la excepción de alto nivel y, a continuación, añade una frase explícita como `"Caused by: ..."` seguida de la traza íntegra del error técnico que provocó la reacción en cadena.

```java
// Clase personalizada de alto nivel
class ConfiguracionInvalidaException extends Exception {
    // Constructor que acepta el mensaje propio y la excepción "causa" originaria
    public ConfiguracionInvalidaException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class Sistema {
    public void cargarConfiguracion() throws ConfiguracionInvalidaException {
        try {
            // Simulamos el fallo de nivel bajo (por ejemplo, disco duro)
            throw new IOException("Permiso denegado para el archivo config.xml");
        } catch (IOException e) {
            // Se encapsula 'e' (IOException) como causa de la nueva excepción
            throw new ConfiguracionInvalidaException(
                "No se pudo inicializar el sistema porque la configuracion falló.", e);
        }
    }
}

```

---
