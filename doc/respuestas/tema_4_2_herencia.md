## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

La **herencia** es un mecanismo estructural de la programación orientada a objetos que permite crear una nueva clase basándose en la definición de una clase preexistente. La relación semántica que se establece es estrictamente **"A es-un B"**. Esto significa que la nueva clase (subclase) representa una especialización de la clase original (superclase). De manera análoga a cómo en C se podrían agrupar datos, la herencia aporta soporte nativo en el lenguaje para definir que un `Artillero` es, inherentemente, un tipo de `Soldado`.

La primera gran implicación es la **herencia de estado y comportamiento**. Esto indica que la subclase absorbe de manera automática todos los atributos y métodos de su clase padre. Se evita así la duplicación de código; no es necesario volver a programar la lógica para almacenar un nombre o la función para saludar, ya que el `Artillero` y el `Zapador` ya las poseen por el simple hecho de heredar de `Soldado`.

La segunda implicación, y quizás la más poderosa, es la **compatibilidad de tipos**. Significa que el compilador permite tratar a cualquier objeto de una subclase exactamente como si fuera del tipo de su superclase. Es asimilable a tratar un puntero a una estructura específica como un puntero a una estructura base. Gracias a esto, se pueden crear arreglos o diseñar funciones que acepten objetos genéricos (la superclase), procesando en su interior los diferentes subtipos sin necesidad de conocer su naturaleza concreta.

```java
// Superclase
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Se presenta el soldado: " + nombre);
    }
}

// Subclase 1
class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() { return cohetes; }
    public void dispararCohete() { System.out.println("¡Fuego!"); }
}

// Subclase 2
class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() { return minas; }
    public void ponerMina() { System.out.println("Mina plantada."); }
}

public class Main {
    public static void main(String[] args) {
        // Uso de la compatibilidad de tipos
        Soldado[] peloton = new Soldado[2];
        peloton[0] = new Artillero("García", 5);
        peloton[1] = new Zapador("López", 10);

        // Se recorre sin importar el tipo concreto, ejecutando el comportamiento heredado
        for (int i = 0; i < peloton.length; i++) {
            peloton[i].saludar();
        }
    }
}
```

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Al instanciar un objeto de una subclase, se ejecutan en cadena los constructores de toda la jerarquía de herencia correspondiente. En concreto, se ejecutan **dos constructores** en el caso de las clases del ejemplo. El orden es estricto: siempre se inicia ejecutando el constructor de la superclase (para inicializar el estado fundamental) y, una vez finalizado, se procede a ejecutar el código del constructor de la subclase (para inicializar el estado específico).

La palabra clave `super` utilizada como instrucción dentro de un constructor sirve para **invocar de forma explícita a un constructor de la clase padre**. Su función es delegar la inicialización de la porción de memoria heredada a la lógica que ya ha sido programada en la superclase, pasándole los argumentos necesarios. Cuando se escribe, tiene la restricción inamovible de ser la primera instrucción dentro del bloque del constructor hijo.

Respecto a su obligatoriedad, si la clase base no posee un constructor visible que carezca de parámetros (un constructor por defecto), **es estrictamente obligatorio** realizar una llamada explícita a `super(...)` en la primera línea del constructor de la subclase. Si no se invoca, el compilador generará un error, dado que no tendría forma de saber con qué valores inicializar el estado que pertenece a la superclase.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Respecto a la disposición en memoria, la respuesta es afirmativa: todos los atributos definidos en la superclase, sin importar si son privados, **forman parte física de la instancia de la subclase en la memoria**. De forma equivalente a cuando en C se anida un `struct` dentro de otro y ocupa espacio de manera contigua, al instanciar un `Zapador`, el bloque de memoria reservado contiene espacio tanto para su atributo propio (`minas`) como para el atributo heredado (`nombre`).

Sin embargo, que dichos datos existan físicamente en el objeto no implica que el código de la subclase posea permisos para acceder a ellos directamente. Debido a las reglas de encapsulación, un atributo declarado como privado en la superclase es totalmente opaco para las clases derivadas. El compilador bloqueará cualquier intento de lectura o escritura directa para proteger la integridad del estado de la clase base.

En el ejemplo proporcionado, aunque un objeto de tipo `Zapador` posee físicamente la cadena de texto del `nombre` en su interior, si se intenta escribir una instrucción como `this.nombre = "Nuevo";` dentro de un método de `Zapador`, el compilador devolverá un error de visibilidad. La única manera en la que el `Zapador` puede interactuar con ese segmento de su propio estado es invocando métodos de acceso (getters o setters) que la clase `Soldado` haya dejado públicos o protegidos.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad a nivel de tipos proporciona una ventaja fundamental en el diseño de software al favorecer una **alta extensibilidad**. Permite que el sistema (como rutinas de procesamiento o bucles) sea escrito basándose exclusivamente en el "contrato" de la superclase genérica. De este modo, el código queda preparado para manipular futuras especializaciones sin necesidad de conocerlas de antemano. Este concepto aplica el *Principio Abierto/Cerrado*, logrando que un programa esté abierto a adquirir nuevas capacidades pero cerrado a la alteración de su código base.

Las implicaciones directas en el mantenimiento son notables. Si en un futuro se requiere agregar una nueva entidad al programa, basta con añadir la clase correspondiente que herede de la superclase. Todas las porciones del código previamente escritas que iteraban u operaban sobre el tipo genérico seguirán funcionando de manera transparente con las nuevas instancias, sin requerir modificaciones ni condicionales añadidos.

```java
// Nueva extensión del código, añadida a posteriori
class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public void curar() { System.out.println("Aplicando curas..."); }
}

public class Main {
    public static void main(String[] args) {
        // El nuevo soldado se introduce al sistema
        Soldado[] peloton = new Soldado[3];
        peloton[0] = new Artillero("García", 5);
        peloton[1] = new Zapador("López", 10);
        peloton[2] = new Medico("Ramírez", 3); // Nueva clase

        // Este bloque de código NO ha requerido ninguna modificación
        // y funciona perfectamente con el nuevo tipo 'Medico'
        for (int i = 0; i < peloton.length; i++) {
            peloton[i].saludar();
        }
    }
}
```

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

En Java es completamente legal y habitual tener una referencia del supertipo apuntando a una instancia real en memoria de un subtipo. No obstante, al realizar llamadas a través de esta referencia genérica, el compilador restringe la visibilidad: **solo se permite invocar los métodos definidos en la superclase**. El compilador se basa en el tipo de la variable (la referencia), no en el tipo del objeto físico, por lo que los métodos específicos del subtipo quedan inaccesibles de forma directa.

El **"upcasting"** es precisamente el proceso de asignar un objeto de un subtipo a una referencia del supertipo. Es un proceso seguro y automático que no requiere sintaxis especial. En contraste, el **"downcasting"** es la operación inversa: forzar una referencia de la superclase para ser interpretada temporalmente como si fuera de un subtipo. Requiere conversión explícita (casteo) y conlleva riesgos, ya que, si el objeto en memoria no resulta ser de ese subtipo específico, el programa lanzará una excepción abortando la ejecución.

Para evitar errores críticos al hacer downcasting, se utiliza el operador **`instanceof`**. Esta herramienta permite evaluar en tiempo de ejecución si un objeto referenciado pertenece verdaderamente a la clase por la que se consulta, asegurando que la posterior conversión de tipos se realice de forma segura.

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] peloton = { new Artillero("García", 5), new Zapador("López", 10) };

        for (int i = 0; i < peloton.length; i++) {
            Soldado soldado = peloton[i]; // Upcasting implícito
            
            // soldado.getCohetes(); // ERROR de compilación: Soldado no tiene este método
            
            // Comprobación segura antes del downcasting
            if (soldado instanceof Artillero) {
                // Downcasting explícito
                Artillero artillero = (Artillero) soldado; 
                System.out.println(artillero.getNombre() + " tiene " + artillero.getCohetes() + " cohetes.");
            }
        }
    }
}
```

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El nivel de acceso **protegido** sirve como un puente entre la privacidad absoluta y la exposición pública. Cuando un atributo o método se declara con este nivel, sigue permaneciendo oculto y bloqueado para el uso del público general, pero se otorga un acceso directo y privilegiado a todas las clases que hereden de él (las subclases). Es un mecanismo diseñado específicamente para que las clases derivadas puedan manipular información interna de sus padres sin tener que exponerla al resto de la aplicación.

En Java, se implementa anteponiendo el modificador `protected` a la declaración de la variable o función. Además, presenta una particularidad en el diseño de este lenguaje: en Java el acceso protegido no solo abre la visibilidad a las subclases (estén donde estén), sino también a cualquier otra clase que comparta el mismo paquete jerárquico. 

```java
class Soldado {
    // Al ser protected, las subclases pueden acceder directamente
    protected String nombre; 

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerMina() { 
        // Acceso directo al atributo de la superclase sin necesidad de getters
        System.out.println("El zapador " + this.nombre + " ha colocado una mina."); 
    }
}
```

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

En el paradigma de la orientación a objetos, el concepto de una clase base universal —una única raíz de la que desciendan directa o indirectamente todos los objetos— es una decisión arquitectónica del diseño de cada lenguaje. Una estructura de árbol único facilita que el ecosistema entero comparta un comportamiento básico estándar y simplifica la creación de colecciones genéricas de datos.

Sin embargo, esto **no ocurre en todos los lenguajes** que permiten el paradigma orientado a objetos. En C++, por ejemplo, no existe ninguna jerarquía universal obligatoria; se pueden crear familias de clases completamente desconectadas entre sí, sin compartir ninguna herencia en común. 

En el caso específico de **Java**, sí existe una clase base absoluta para todo el sistema: la clase `Object` (ubicada en `java.lang.Object`). Si al programar una clase nueva el desarrollador no indica que herede de ninguna otra clase de forma explícita, el compilador inyecta automáticamente de forma invisible una herencia hacia `Object`. Esto asegura que absolutamente toda instancia en un programa Java posea, de base, métodos comunes como `equals()`, `toString()` o `hashCode()`.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La **herencia múltiple** es una característica avanzada presente en algunos lenguajes de programación mediante la cual una subclase puede heredar el estado y comportamiento de dos o más superclases simultáneamente. Resulta útil para modelar entidades que poseen naturalezas combinadas de distintos dominios. Sin embargo, introduce severos conflictos estructurales, siendo el más conocido el "problema del diamante": si dos superclases poseen un método con exactamente la misma firma o atributos con el mismo nombre, el compilador debe resolver ambigüedades respecto a cuál versión hereda la clase hija.

En **Java, la herencia múltiple de clases no existe**. Las restricciones de diseño del lenguaje establecen que una clase únicamente puede heredar de manera directa de una sola superclase. Esta prohibición se introdujo deliberadamente en el diseño temprano del lenguaje para eliminar de raíz los problemas de ambigüedad y mantener el modelo de relaciones más limpio y predecible que en C++.

Como alternativa para compensar esta ausencia, Java introduce el mecanismo de las **interfaces**. Las interfaces representan contratos puramente abstractos (comportamientos sin estado o implementación asociada). Aunque una clase no puede adquirir atributos o implementaciones base de varias fuentes simultáneas, sí tiene permitido declarar la implementación de múltiples interfaces, logrando compatibilidad de tipos polimórfica frente a diferentes abstracciones sin riesgo de colisiones en la estructura de memoria.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

Efectivamente, al basarse el sistema de manejo de errores en objetos, es sencillo aprovechar las bondades de la herencia para definir jerarquías propias de errores. Para crear una excepción que sea categorizada como "no controlada" (unchecked), es decir, aquellas que el compilador de Java no obliga a envolver en bloques `try/catch` de forma forzosa, es necesario heredar de la clase base `RuntimeException`.

El principal beneficio de construir excepciones como objetos es aplicar los conocimientos de composición. Una excepción no tiene por qué limitarse a ser solo un mensaje de texto plano; puede albergar referencias a los objetos involucrados en la anomalía (como la entidad `Usuario` específica). De esta manera, al ser capturada en niveles superiores, la rutina de manejo de errores posee acceso al estado completo que propició el fallo, permitiendo emitir bitácoras mucho más ricas o emprender acciones correctivas automáticas concretas.

```java
// Se asume la existencia previa de una clase Usuario
class Usuario {
    private String id;
    public Usuario(String id) { this.id = id; }
    public String getId() { return id; }
}

// Excepción no controlada (hereda de RuntimeException)
public class UsuarioNoEncontradoException extends RuntimeException {
    
    // Composición: la excepción contiene datos del contexto
    private Usuario usuario;

    // Constructor base
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje);
        this.usuario = usuario;
    }

    // Sobrecarga del constructor para incluir una causa subyacente (encadenamiento)
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario, Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

Emplear la herencia exclusivamente como un atajo técnico para reciclar funciones o datos se considera un defecto de diseño arquitectónico grave. La herencia establece una relación semántica extremadamente estricta y lógica que dicta que "A es-un B". Si se emplea la herencia solo para acceder al código de otra clase (por ejemplo, hacer que un `Coche` herede de una clase `Motor` para poder acceder al método `girarEje()`), se corrompe el sentido natural del modelo, generando esquemas engañosos y poco intuitivos.

Desde una perspectiva estructural, la herencia introduce el **acoplamiento más alto posible** entre dos bloques de código. La clase derivada se vuelve íntimamente ligada a los pormenores internos de su padre. Como resultado, las modificaciones aplicadas a la clase base —incluso en porciones que la subclase no utiliza directamente— pueden desencadenar fallos impredecibles en cadena hacia toda la jerarquía de herencia, un fenómeno conocido como "el problema de la clase base frágil".

Sumado a todo esto, hay que recordar la limitación intrínseca del lenguaje: Java restringe a una única superclase el uso de la palabra `extends`. Desperdiciar esa única carta en una relación que solo busca aprovechar la reutilización de unas cuantas líneas de código, impedirá en un futuro que dicha clase herede de un modelo taxonómico que verdaderamente defina su naturaleza y propósito real.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

Este principio de diseño se prioriza debido a que la composición genera sistemas informáticos notablemente **más flexibles y dinámicos**. La herencia es una estructura completamente estática que queda grabada en piedra durante la compilación del código. Por el contrario, la composición se basa en una relación de "A tiene-un B", ensamblando componentes que interactúan en memoria. Esta forma de construir permite, en tiempo de ejecución, intercambiar el comportamiento funcional de un objeto al cambiar dinámicamente los componentes que lo integran.

Además, la composición favorece de manera natural la creación de clases pequeñas, concretas y altamente cohesivas. En lugar de desarrollar monolíticas superclases abultadas con decenas de métodos con el objetivo de compartirlos, el sistema se divide en pequeñas herramientas independientes. Cada módulo encapsulado puede ser sometido a pruebas, depurado y perfeccionado de manera autónoma sin correr el riesgo de propagar errores hacia una descendencia hereditaria.

La reducción del acoplamiento es quizás el pilar más importante. La clase que alberga el componente interactúa con este estrictamente a través de sus métodos públicos declarados. Esto otorga al desarrollador la libertad de modificar internamente y reestructurar a fondo la clase componente con la absoluta certeza de que no afectará a los sistemas dependientes de ella, garantizando mantenimientos futuros más seguros y predecibles.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

Esta conocida afirmación, promovida extensamente en la bibliografía sobre patrones de diseño, hace referencia a que la herencia fuerza a las clases derivadas a **depender fuertemente de los detalles concretos de implementación de sus superclases**. Cuando se utiliza una clase mediante composición regular (código cliente), el contrato se sella al usar métodos públicos; se desconoce el mecanismo interno. En la herencia, por el contrario, una subclase se inyecta y superpone sobre el esqueleto lógico preexistente del padre.

Frecuentemente, el desarrollador que codifica una subclase se ve obligado a conocer el orden exacto en el que la clase padre invoca a sus propios métodos internos o cómo altera su estado oculto. Si en una versión futura la superclase decide cambiar la forma en la que resuelve una tarea internamente —aunque su firma pública siga siendo idéntica—, una subclase que había sobreescrito algún método intermedio verá quebrado por completo su funcionamiento esperado.

De este modo, se dice que la encapsulación "se rompe" a través del propio vector de la herencia. La coraza protectora que impide que los detalles de una implementación filtren dependencias queda diluida entre los eslabones de la cadena familiar, obligando a los arquitectos de software a ser extremadamente cuidadosos y precavidos al diseñar o evolucionar jerarquías prolongadas.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

El primer enfoque consiste en diseñar a través de herencia estática. Al observar información en común, se infiere y extrae una abstracción general, una superclase superior en jerarquía que ampara a ambas entidades bajo una relación taxonómica directa. De este modo, un Estudiante "es" una Persona en toda regla.

```java
// Alternativa 1: Herencia
class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class EstudianteH extends Persona {
    private String matricula;

    public EstudianteH(String dni, String nombre, String matricula) {
        super(dni, nombre);
        this.matricula = matricula;
    }
}

class TrabajadorH extends Persona {
    private String empresa;

    public TrabajadorH(String dni, String nombre, String empresa) {
        super(dni, nombre);
        this.empresa = empresa;
    }
}
```

El segundo enfoque abandona la vinculación rígida de familias de tipos y la reemplaza por la inclusión de componentes como herramientas. La información identificativa se aisla en un bloque utilitario independiente. De este modo, `Estudiante` no hereda de nadie, sino que delega la custodia de los datos comunes a un ente separado que "tiene" alojado en su propio interior.

```java
// Alternativa 2: Composición
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class EstudianteC {
    private DatosPersonales identidad;
    private String matricula;

    // Se inyecta la dependencia por el constructor
    public EstudianteC(DatosPersonales identidad, String matricula) {
        this.identidad = identidad;
        this.matricula = matricula;
    }
}

class TrabajadorC {
    private DatosPersonales identidad;
    private String empresa;

    // Se inyecta la dependencia por el constructor
    public TrabajadorC(DatosPersonales identidad, String empresa) {
        this.identidad = identidad;
        this.empresa = empresa;
    }
}
```