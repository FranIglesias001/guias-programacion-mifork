# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?
prof:
Objetivo: facilita la extension de los programas

facilita que esta extensión se haga creando código nuevo frente a editar codigo existente

Mencanismo en Java:
- Sobreescritura
- Clases abstractas y métodos abstractos
- Interfaces:
    - Todos los métodos son abstractos, es decir, sin código ( A partir de cierta versión de java, se deja meter una implementación "default").
    - No tienen estado.
    - Una clase puedo implementar más de una interfaz.
### Respuesta

El polimorfismo es un pilar fundamental de la programación orientada a objetos que permite tratar objetos de distintas clases derivadas como si fueran objetos de su clase base. Su nombre proviene del griego y significa "múltiples formas", lo que en programación se traduce en la capacidad de una misma variable de referencia (por ejemplo, un puntero o referencia a la clase padre) para apuntar a objetos de diferentes subclases y comportarse de manera distinta según el objeto real al que apunte en tiempo de ejecución. Sirve principalmente para escribir código más genérico, flexible y mantenible, ya que es posible diseñar sistemas que trabajen con tipos abstractos sin preocuparse por las implementaciones específicas de cada variante.

Por otro lado, la sobreescritura (o *overriding* en inglés) de métodos es el mecanismo que hace posible el polimorfismo de comportamiento. Consiste en redefinir en una clase hija un método que ya ha sido implementado en su clase padre. Para que se considere sobreescritura, el método en la subclase debe tener exactamente el mismo nombre, el mismo tipo de retorno y la misma lista de parámetros que el método de la clase base. De este modo, cuando se invoca el método a través de una referencia genérica, se ejecuta la versión específica y redefinida por el objeto instanciado.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La ligadura dinámica (también conocida como enlace tardío o *late binding*) es el proceso mediante el cual el sistema determina, durante el tiempo de ejecución del programa, qué bloque de código debe ejecutarse al realizar la llamada a un método. En lenguajes procedimentales como C, las llamadas a funciones se resuelven en tiempo de compilación (ligadura estática), pero en la programación orientada a objetos, si existe sobreescritura, el compilador no siempre puede saber de antemano de qué tipo exacto será el objeto al que apunta una referencia. La relación con el polimorfismo es directa: la ligadura dinámica es el mecanismo interno que permite que el polimorfismo funcione, asegurando que se llame al método de la clase real del objeto y no al método de la clase de la variable que lo referencia.

La forma de aplicar la ligadura dinámica depende drásticamente del lenguaje de programación empleado. En C++, por defecto, los métodos utilizan ligadura estática por cuestiones de rendimiento. Para habilitar el polimorfismo y la ligadura dinámica en C++, es obligatorio declarar explícitamente los métodos en la clase base con la palabra clave `virtual`. Si no se incluye esta directiva, una referencia de tipo padre siempre ejecutará el método del padre, sin importar si el objeto subyacente es un hijo.

En contraste, Java invierte este enfoque: por defecto, todos los métodos de instancia (que no sean privados ni estáticos) utilizan ligadura dinámica automáticamente. No es necesario añadir ninguna palabra clave para lograr el comportamiento polimórfico. En cuanto a Python, al ser un lenguaje de tipado dinámico que utiliza el concepto de *duck typing* (tipado pato), la ligadura es inherentemente dinámica siempre. En Python ni siquiera es necesaria una jerarquía de herencia formal para invocar métodos polimórficos; basta con que los diferentes objetos posean un método con el mismo nombre en el momento de la ejecución.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

A continuación, se define la jerarquía básica solicitada. Se crea una clase base `Soldado` con un comportamiento predeterminado, y dos clases derivadas: `Artillero`, que hereda el comportamiento tal cual (al no sobreescribir el método), y `Zapador`, que proporciona su propia implementación mediante la sobreescritura. 

```java
class Soldado {
    public void saludar() {
        System.out.println("¡Señor, sí, señor! Soldado listo.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("¡Camino despejado de explosivos, señor!");
    }
}

class Artillero extends Soldado {
    // Hereda el comportamiento predeterminado de Soldado
}
```

Para ilustrar el polimorfismo en acción, se puede agrupar a diferentes tipos de soldados en una estructura de datos común, como un arreglo, cuyo tipo base es `Soldado`. Al iterar sobre este arreglo, se puede invocar el método `saludar()` sin necesidad de comprobar de qué tipo específico es cada elemento. La máquina virtual de Java se encargará de utilizar la ligadura dinámica para ejecutar la versión correcta del método.

```java
public class Main {
    public static void main(String[] args) {
        // Creación de un array polimórfico
        Soldado[] peloton = new Soldado[3];
        peloton[0] = new Soldado();
        peloton[1] = new Zapador();
        peloton[2] = new Artillero();

        // Recorrido utilizando referencias del tipo base
        for (int i = 0; i < peloton.length; i++) {
            peloton[i].saludar();
        }
    }
}
```

Al ejecutar el código anterior, la salida será la predeterminada para la primera y tercera posición del arreglo, pero mostrará el mensaje modificado para la segunda posición, demostrando que el comportamiento polimórfico se adapta al tipo dinámico de cada instancia.

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Sí, es completamente posible y muy habitual invocar el método de la clase base desde un método sobreescrito. Esto se hace cuando no se desea reemplazar por completo el comportamiento original, sino más bien extenderlo, aprovechando la lógica que ya está definida en la superclase para luego añadir la funcionalidad específica de la subclase. De este modo, se fomenta la reutilización de código y se evitan duplicidades.

En Java, la palabra clave utilizada para acceder a los miembros (métodos o atributos) de la clase base inmediata es `super`. Su funcionamiento es análogo al uso de `this`, pero mientras `this` hace referencia a la instancia actual de la clase en la que se está, `super` fuerza la llamada a la implementación del padre, saltándose la regla habitual de la ligadura dinámica en ese momento concreto.

```java
class Zapador extends Soldado {
    @Override
    public void saludar() {
        // Se invoca el comportamiento de la clase base
        super.saludar(); 
        // Se añade el comportamiento específico de la subclase
        System.out.println("¡ZAPADOR A SUS ORDENES!");
    }
}
```

Si se volviera a ejecutar el array del ejercicio anterior con esta nueva implementación, el `Zapador` imprimiría primero "¡Señor, sí, señor! Soldado listo." e inmediatamente después "¡ZAPADOR A SUS ORDENES!", demostrando la concatenación de ambos comportamientos mediante el uso explícito de `super.saludar()`.

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Al sobreescribir un método en Java existen reglas estrictas que deben cumplirse para que el compilador lo reconozca como tal. Los tipos, número y orden de los parámetros deben ser exactamente idénticos a los del método de la clase base. En cuanto al tipo de retorno, tradicionalmente debía ser el mismo, pero desde versiones modernas de Java se permiten los "tipos de retorno covariantes"; esto significa que el método sobreescrito puede devolver una subclase del tipo que devolvía el método original. Además, el modificador de acceso no puede ser más restrictivo que el original (por ejemplo, si en el padre es `protected`, en el hijo puede ser `protected` o `public`, pero nunca `private`).

Es fundamental distinguir la sobreescritura (*overriding*) de la sobrecarga (*overloading*). La sobrecarga ocurre cuando en una misma clase (o mediante herencia) existen múltiples métodos con el mismo nombre, pero con una lista de parámetros distinta (diferente número o tipo). La sobrecarga se resuelve en tiempo de compilación (ligadura estática) evaluando los argumentos pasados, mientras que la sobreescritura requiere exactamente los mismos parámetros y se resuelve en tiempo de ejecución (ligadura dinámica) evaluando el tipo de objeto.

La anotación `@Override` es una marca que se coloca justo encima de la declaración de un método en la clase hija. Su propósito es instruir al compilador para que verifique que efectivamente se está sobreescribiendo un método de la clase padre. Es altamente recomendable usarla siempre porque evita errores sutiles: si por accidente se escribe mal el nombre del método o se cambia un parámetro, sin `@Override` el compilador interpretará que se está creando un método nuevo (sobrecarga) y no mostrará error, pero el polimorfismo no funcionará. Con la anotación, el compilador fallará inmediatamente advirtiendo del error.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Efectivamente, en Java se hace uso del polimorfismo prácticamente desde el primer día de aprendizaje, incluso antes de que el programador sea consciente de estar diseñando jerarquías de herencia complejas. Esto se debe a la arquitectura fundamental del lenguaje: todas las clases en Java, sin excepción, heredan directa o indirectamente de una clase raíz llamada `java.lang.Object`.

La clase `Object` proporciona implementaciones predeterminadas para varios métodos fundamentales, entre los que destacan `toString()` y `equals(Object obj)`. Cuando un programador crea una clase propia, como `Persona` o `Factura`, y define su propia versión de `toString()` para devolver una representación legible del estado del objeto, está empleando sobreescritura. 

Por lo tanto, al sobreescribir estos métodos, se está utilizando el polimorfismo. Funciones internas de Java, como el método `System.out.println()`, reciben parámetros de tipo `Object`. Al pasar un objeto recién creado a estas funciones de la API estándar, éstas invocan dinámicamente el método `toString()` sobreescrito específico de la clase del usuario, confirmando que el comportamiento polimórfico y el enlace tardío están en acción constante dentro del ecosistema de Java.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una clase abstracta es una clase que sirve como plantilla o modelo general para otras clases derivadas, pero que está incompleta por sí misma. Su propósito principal es agrupar atributos y métodos comunes, estableciendo un "contrato" que las subclases deben cumplir. La característica más importante de una clase abstracta es que es imposible crear instancias directas de ella (no se puede usar `new` con su constructor), ya que su naturaleza exige que deba ser heredada y completada por clases concretas.

Un método abstracto, por su parte, es un método que se declara en una clase abstracta especificando su firma (nombre, parámetros y tipo de retorno), pero sin proporcionar ningún cuerpo o implementación (no lleva llaves `{}`). La responsabilidad de implementar este método recae obligatoriamente sobre las subclases concretas. En Java, la palabra clave `abstract` se debe colocar tanto en la firma del método sin implementación como en la declaración de la clase que lo contiene.

A continuación, se reestructura el ejemplo del `Soldado` para incorporar abstracción. Al añadir un método abstracto `atacar()`, la clase `Soldado` debe declararse abstracta. Las clases hijas quedan entonces obligadas a implementar cómo atacan.

```java
// Se requiere 'abstract' en la definición de la clase
abstract class Soldado {
    public void saludar() {
        System.out.println("¡Señor, sí, señor! Soldado listo.");
    }
    
    // Se requiere 'abstract' en la firma. Termina en punto y coma.
    public abstract void atacar(); 
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El Zapador coloca explosivos C4.");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("El Artillero dispara ráfagas de ametralladora.");
    }
}
```

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?
prof:
final en clases: prohibe heredar
final en métodos: prohibe sobreescribir
### Respuesta

La palabra clave `final` en Java actúa como un mecanismo restrictivo diseñado para detener la herencia y la modificación de comportamientos. Cuando se aplica a la declaración de una clase (por ejemplo, `public final class MiClase`), se impide absolutamente que ninguna otra clase pueda heredar de ella. Es decir, la clase no puede tener subclases. Cuando se aplica a un método dentro de una clase, significa que dicho método no puede ser sobreescrito por ninguna clase hija, asegurando que su comportamiento original permanezca inalterable a lo largo de toda la jerarquía de herencia restante.

Su relación con el polimorfismo es antagónica: el uso de `final` se emplea específicamente para limitar o prohibir el polimorfismo. Los diseñadores de software utilizan esta palabra clave cuando por motivos de seguridad o integridad estructural necesitan garantizar que un objeto se comporte siempre de una manera predecible, sin el riesgo de que una subclase malintencionada o mal diseñada altere sus operaciones críticas a través de la sobreescritura. Al hacer un método `final`, el compilador también puede optimizar las llamadas mediante ligadura estática, ya que sabe con certeza que no habrá variaciones en tiempo de ejecución.

En la API estándar de Java, el ejemplo más célebre de una clase `final` es la clase `String`. Esta decisión de diseño garantiza que las cadenas de texto sean inmutables y completamente seguras. Si alguien pudiera crear una subclase de `String` y sobreescribir sus métodos polimórficamente, se podría corromper la integridad de infinidad de componentes del núcleo de Java que dependen de que los textos se comporten exactamente como dicta la especificación original. Otras clases como `Math` y las clases envolventes o *wrappers* (`Integer`, `Double`) también son de tipo `final`.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

Las interfaces en Java son un mecanismo fundamental para definir "contratos" de comportamiento sin dictar cómo deben implementarse. En su forma más clásica, una interfaz actúa como un tipo completamente abstracto que agrupa declaraciones de métodos (sin cuerpo) y constantes. Cualquier clase que decida implementar una interfaz se compromete de forma vinculante a aportar el código para cada uno de los métodos definidos en ella. Esto permite un nivel altísimo de desacoplamiento y facilita un polimorfismo muy flexible, donde clases que no guardan relación de parentesco jerárquico directo pueden ser tratadas bajo el mismo tipo si implementan la misma interfaz.

Se parecen a las clases abstractas en que ambas no pueden ser instanciadas y ambas obligan a las subclases a implementar métodos. Sin embargo, su propósito de diseño es distinto. Una clase abstracta está pensada para crear una jerarquía taxonómica (un "es un", por ejemplo, el `Zapador` *es un* `Soldado`), compartiendo estado (variables de instancia) y métodos con cuerpo. Las interfaces establecen capacidades o roles (un "es capaz de", por ejemplo, `Volador`, `Guardable`), y tradicionalmente carecían de estado interno. A partir de Java 8, se introdujeron los métodos *default* en las interfaces, difuminando ligeramente la línea, pero la separación conceptual se mantiene.

La diferencia más crucial en Java es que una clase solo puede heredar (extender) de una única clase padre (sea abstracta o no), para evitar la complejidad de la herencia múltiple de estado. No obstante, una clase sí puede implementar múltiples interfaces simultáneamente, separándolas por comas. Esto permite a Java simular los beneficios de la herencia múltiple en cuanto a tipos, dotando a una clase de varios comportamientos diferentes procedentes de fuentes diversas sin los problemas de ambigüedad estructural.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

Para resolver este escenario, se establece una clase abstracta `Punto` que define el contrato `calcularDistanciaA`. Las implementaciones concretas `Punto2D` y `Punto3D` deben comprobar, utilizando el operador `instanceof`, si el punto que reciben como argumento es de su misma dimensión. Si es compatible, se realiza un *downcasting* seguro para acceder a las coordenadas específicas de la dimensión correspondiente y aplicar la fórmula matemática pertinente.

```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto p);
}

class Punto2D extends Punto {
    private double x, y;
    public Punto2D(double x, double y) { this.x = x; this.y = y; }

    @Override
    public double calcularDistanciaA(Punto p) {
        if (p instanceof Punto2D) {
            Punto2D otro = (Punto2D) p; // Downcasting
            return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
        }
        throw new IllegalArgumentException("El punto debe ser 2D");
    }
}

class Punto3D extends Punto {
    private double x, y, z;
    public Punto3D(double x, double y, double z) { this.x = x; this.y = y; this.z = z; }

    @Override
    public double calcularDistanciaA(Punto p) {
        if (p instanceof Punto3D) {
            Punto3D otro = (Punto3D) p; // Downcasting
            return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2) + Math.pow(otro.z - this.z, 2));
        }
        throw new IllegalArgumentException("El punto debe ser 3D");
    }
}
```

La verdadera potencia del polimorfismo y de este diseño se evidencia en la clase `Linea`. Al depender de la abstracción `Punto`, la línea desconoce si está operando en el plano o en el espacio. Simplemente se encarga de guardar las referencias abstractas y delegar el cálculo de la longitud al propio método polimórfico. Esto garantiza que si en el futuro se añade un `Punto4D`, la clase `Linea` seguirá funcionando a la perfección sin requerir modificaciones.

```java
class Linea {
    private Punto inicio;
    private Punto fin;

    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double obtenerLongitud() {
        // Ligadura dinámica: no importa si inicio es 2D o 3D,
        // ejecutará el cálculo de distancia correcto.
        return inicio.calcularDistanciaA(fin);
    }
}
```

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta

La herencia de interfaces en Java es un mecanismo que permite a una interfaz derivar de otra, expandiendo el contrato inicial con nuevas exigencias. Al igual que las clases utilizan la palabra clave `extends` para heredar de otra clase, las interfaces utilizan esta misma palabra clave para heredar de otras interfaces. Como resultado, cualquier clase concreta que implemente la interfaz hija o sub-interfaz estará obligada a implementar no solo los métodos declarados en ella, sino también todos los métodos heredados de la interfaz o interfaces padre.

Es importante destacar que, aunque en Java no existe herencia múltiple para las clases (para evitar el llamado "Problema del Diamante"), sí existe y está permitida la herencia múltiple de interfaces. Una interfaz puede extender múltiples interfaces utilizando una lista separada por comas (por ejemplo, `interface C extends A, B`). Dado que tradicionalmente las interfaces solo definen contratos sin implementar estados ni lógicas que puedan entrar en conflicto, el compilador puede gestionar estas herencias múltiples sin ambigüedades.

A continuación, se ilustra la herencia de interfaces solicitada. Primero, se define una interfaz básica de solo lectura, y a partir de ella, se genera una interfaz más especializada que hereda su contrato pero añade capacidades de escritura y eliminación.

```java
// Interfaz base
interface Fichero {
    String leerContenido();
}

// Herencia de interfaz mediante 'extends'
interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminarFichero();
}

// Clase que implementa la sub-interfaz debe satisfacer todo el contrato combinado
class DocumentoTexto implements FicheroEscribible {
    @Override
    public String leerContenido() {
        return "Leyendo contenido de texto...";
    }

    @Override
    public void escribirContenido(String contenido) {
        System.out.println("Escribiendo: " + contenido);
    }

    @Override
    public void eliminarFichero() {
        System.out.println("Fichero de texto eliminado.");
    }
}
```

Prof:
P