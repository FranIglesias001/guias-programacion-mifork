Aquí tienes las respuestas al cuestionario, adaptadas a los conocimientos previos especificados y redactadas en tono impersonal.

# Tema 4.1. Composición

## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

En el lenguaje C, la composición se logra agrupando tipos de datos más simples, ya sean primitivos u otras estructuras, dentro de un nuevo `struct`. Esto permite modelar entidades más complejas mediante la relación "tiene-un" (o *has-a* en inglés). Al definir una estructura dentro de otra, la estructura contenedora aloja directamente los datos de las estructuras contenidas en su propio bloque de memoria.

Para el caso planteado, se requiere primero una estructura `Punto` que almacene las coordenadas espaciales. Posteriormente, se define la estructura `Linea` que contiene exactamente dos instancias de `Punto`. Para el cálculo matemático de la separación espacial, se emplea la fórmula de la distancia euclidiana: 

$$d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$

A continuación se presenta el código correspondiente en C, separando la lógica matemática en funciones independientes que reciben las estructuras pertinentes:

```c
#include <stdio.h>
#include <math.h>

struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};

double calcular_distancia(struct Punto a, struct Punto b) {
    return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
}

double calcular_longitud(struct Linea l) {
    return calcular_distancia(l.p1, l.p2);
}

```
prof:
```c
struct Punto {
    double x;
    double y;
}

struct Linea {
    struct Punto p1; // Composición
    struc Punto p2; // Una Linea tiene dos Puntos
}

double distancia (struct Punto p1, struct Punto p2) {
    //...
}

double longitud (struct Linea l) {
    return distancia (l.p1, l.p2);
}
```
---

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.

### Respuesta

La transición al paradigma de orientación a objetos en Java permite encapsular tanto el estado (los datos) como el comportamiento (las funciones) dentro de las propias clases. A diferencia de C, donde las funciones operan sobre estructuras pasivas, en Java son los propios objetos los que exponen métodos para interactuar con su información.

Para lograr la inmutabilidad solicitada, se deben aplicar los principios de ocultación de información. Esto se consigue marcando los atributos internos como `private` (para evitar el acceso desde el exterior) y como `final` (para garantizar que su valor solo se pueda asignar una vez, durante la inicialización en el constructor). Al carecer de métodos *setters*, el estado del objeto queda protegido e inalterable durante toda su existencia.

El siguiente código muestra la implementación. Obsérvese cómo el método `longitud` de la clase `Linea` delega la responsabilidad del cálculo matemático al método `distancia` de la clase `Punto`, promoviendo un diseño más modular:

```java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}

```

---

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad es un concepto del modelado de software que indica la cantidad de instancias de una clase que pueden (o deben) estar relacionadas con una instancia de otra clase. Sirve para definir los límites y restricciones de una relación estructural, especificando si es una relación de uno a uno, uno a muchos, o muchos a muchos, así como los valores mínimos y máximos permitidos.

En el ejemplo de `Linea` y `Punto`, la multiplicidad se puede analizar de forma bidireccional. Desde la perspectiva de la línea hacia el punto (de `Linea` a `Punto`), la multiplicidad es **1 a 2**. Esto significa que exactamente una instancia de `Linea` está compuesta invariablemente por exactamente dos instancias de `Punto` (sus extremos). No existe una línea en este diseño con uno o con tres puntos.

Por otro lado, observando la relación en sentido inverso (de `Punto` a `Linea`), la multiplicidad depende de cómo se utilicen las coordenadas en el sistema completo, pero habitualmente es de **1 a 0..N** (cero a muchos). Es decir, un mismo objeto `Punto` podría teóricamente compartirse y formar parte de varias líneas simultáneamente, o bien existir de forma independiente sin pertenecer a ninguna línea.


prof:
establece con cuantas instancias de tipo B se relaciona 1 instancia de tipo A, como mínimo y como máximo. Se puede ver en ambos sentidos: de A a B y de B y A.
Ej: entre Punto y Linea (Notación UML)
                    ______________                                                   ______________
Atributos           |  P1.Punto  | 0.*                                           2.2 |x: double   |
Atributs            |  p2.Punto  |---------------------------------------------------|y: double   |
                    --------------                                                   --------------   
Métodos             |            |                                                   |            |
                    --------------                                                   --------------
---

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La diferencia entre estos dos tipos de composición radica en la fuerza del vínculo y, fundamentalmente, en quién es el propietario responsable del ciclo de vida de los objetos implicados. En una **composición fuerte**, las partes constituyentes carecen de sentido o existencia fuera del objeto contenedor. Si el objeto principal se destruye, sus partes lógicamente también deberían dejar de existir.

En contraste, la **composición débil** representa una relación donde el objeto contenedor usa o agrupa a otros objetos, pero estos últimos tienen un ciclo de vida independiente. Es decir, los objetos contenidos existen por sí mismos en el sistema y pueden seguir siendo utilizados por otras entidades incluso si el objeto que los agrupaba desaparece.

En la terminología estándar del diseño orientado a objetos, a la composición fuerte se la denomina simplemente **"Composición"**. A la composición débil se la suele clasificar como **"Agregación"** (si existe una relación de todo/parte pero con vidas independientes) o de manera más general como **"Asociación"** (si es simplemente una relación de uso estructural entre entidades sin jerarquía estricta).


Prof:
=> Fuerte: El objeto contenido vive en función del objeto que lo contiene. No existe antes, ni vive más allá. Por tanto el ciclo de vida no es independiente. El contenedor controla el ciclo de vida del contenido. P.ej:

=> Débil. Los ciclos de vida son independientes. P.ej: Punto viven independentes de linea.
---

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

En estos escenarios, se trata de una relación de **dependencia**, no de composición. Una dependencia ocurre cuando una clase utiliza a otra para realizar una tarea específica en un momento puntual, pero no almacena una referencia a ella como parte de su estado permanente a lo largo del tiempo.

La dependencia representa una relación del tipo "usa-un" (o *uses-a*). Se evidencia comúnmente en el código cuando el nombre de una clase aparece temporalmente dentro de un método, ya sea como el tipo de un parámetro recibido, el tipo de retorno de la función, o como una variable local instanciada fugazmente.

Para que exista composición (o asociación/agregación), la relación estructural debe reflejarse en los atributos (variables de instancia) de la clase. Es decir, la clase debe poseer a la otra como parte de su propia estructura interna persistente, manteniendo la referencia viva entre múltiples invocaciones a sus métodos.

Prof:
Dependencia:
Ej:
- A recibe parametros de B, es un método.
- A devuelbe B en algun método.
- A hace new de B.
- A llama a métodos de B

NO es comoposición porque:
- A no tiene atributos de B.
Usar no es componerse con

---

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

Para implementar una **composición fuerte**, la clase `Linea` debe encargarse de crear las instancias de `Punto` por sí misma, asegurando que nadie más tenga referencias a esos puntos. En lugar de recibir objetos `Punto` ya creados, el constructor de `Linea` recibe las coordenadas primitivas y hace el `new` internamente.

```java
// Composición Fuerte
public class LineaFuerte {
    private final Punto p1;
    private final Punto p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}

```

Para una **composición débil**, la clase `Linea` se limita a recibir por parámetro objetos `Punto` que ya fueron creados externamente por otra parte del programa. Esto permite que los mismos puntos puedan ser reutilizados, compartidos con otras líneas o mantener su ciclo de vida aunque la línea desaparezca.

```java
// Composición Débil (Agregación)
public class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1; // Se guardan las referencias externas
        this.p2 = p2;
    }
}

```

---

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En los lenguajes carentes de gestión automática de memoria, como C o C++, el programador debe invocar explícitamente rutinas como `free()` o usar operadores como `delete` para liberar la memoria de las partes cuando el objeto contenedor se destruye. Sin embargo, Java funciona de manera fundamentalmente distinta gracias a un componente integrado en su máquina virtual llamado Recolector de Basura (*Garbage Collector*).

En Java, no existe la destrucción explícita de objetos. El ciclo de vida de un objeto termina cuando ya no existen referencias apuntando hacia él en el flujo de ejecución del programa. En una composición fuerte, los objetos internos (como `Punto`) solo están referenciados por el contenedor (como `Linea`).

Por consiguiente, cuando el objeto `Linea` deja de ser referenciado y se vuelve inaccesible, sus atributos internos también pierden su única vía de acceso. Al detectar que los objetos `Punto` han quedado sin referencias activas (huérfanos), el Recolector de Basura actúa de forma automática y asíncrona liberando la memoria que ocupaban, garantizando su "destrucción" sin requerir código explícito por parte del desarrollador.


prof:
Cuando Linea es inaccesible, según el codigo anteior de lina composicion fuerte, sus Punto también serán mas accesibles, luego son basura no más tarde de la Linea que los contiene.
---

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

Se plantea un modelo donde se gestiona un array estático de capacidad fija manteniendo un control riguroso de la cantidad real de elementos ingresados. Esta implementación requiere garantizar que la referencia al director se mantenga válida y siempre coincida con algún elemento presente en la estructura de almacenamiento.

Dado que la manipulación de arrays primitivos puede ser delicada al añadir o eliminar elementos intermedios (requiriendo desplazamiento de posiciones), las operaciones modificadoras deben validar estrictamente la lógica de la invariante: no se puede eliminar a un profesor si este tiene el rol de director, ni se puede asignar a un director que no esté previamente matriculado en el departamento.

A continuación, se muestra el código empleando arrays encapsulados y excepciones de Java (`IllegalArgumentException` e `IllegalStateException`) para rechazar operaciones inválidas:

```java
public class Profesor { 
    // Clase simplificada para el ejemplo
}

public class Departamento {
    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) throw new IllegalArgumentException("Se requiere un director inicial.");
        this.profesores = new Profesor[50];
        this.numProfesores = 0;
        this.director = directorInicial;
        addProfesor(directorInicial);
    }

    public void addProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("El profesor no puede ser nulo.");
        if (numProfesores >= profesores.length) throw new IllegalStateException("Departamento lleno.");
        profesores[numProfesores] = p;
        numProfesores++;
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) throw new IllegalArgumentException("Posición inválida.");
        if (profesores[pos] == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento.");
        }
        // Desplazar elementos para no dejar huecos
        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) throw new IllegalArgumentException("Posición inválida.");
        return profesores[pos];
    }

    public void setDirector(Profesor nuevoDirector) {
        boolean pertenece = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                pertenece = true;
                break;
            }
        }
        if (!pertenece) {
            throw new IllegalArgumentException("El nuevo director debe pertenecer al departamento.");
        }
        this.director = nuevoDirector;
    }
}

```
prof :
exlpicitas que se ven.
implicitas, que no se ven pero están ahi.
---

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

La interfaz `List` del API de Colecciones de Java (comúnmente implementada mediante `ArrayList`) proporciona estructuras dinámicas que gestionan automáticamente su tamaño interno y el desplazamiento de elementos. Al emplearla, se ahorra la necesidad de mantener variables contadoras (como `numProfesores`), el establecimiento de límites fijos rígidos (`new Profesor[50]`), y la lógica manual de los bucles para mover elementos y compactar vacíos en operaciones de eliminación.

Si existiera un método que devolviese la colección completa de profesores (por ejemplo `List<Profesor> getProfesores()`), retornar directamente la referencia a la variable `listaProfesores` interna sería una grave violación de la encapsulación. Cualquier clase externa que llamase a ese método obtendría acceso directo a la estructura, y podría usar métodos como `.add()` o `.clear()` sobre ella, alterando el estado del departamento desde fuera, sorteando los controles e invalidando las invariantes impuestas.

Para resolver este problema, en lugar de devolver la lista original, se debe retornar una abstracción segura. La forma más adecuada en Java moderno es usar el método `Collections.unmodifiableList(listaProfesores)`, que devuelve una vista de solo lectura de la colección, lanzando una excepción si alguien intenta modificarla. Otra alternativa es retornar una copia defensiva: `new ArrayList<>(listaProfesores)`.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class DepartamentoList {
    private final List<Profesor> profesores;
    private Profesor director;

    public DepartamentoList(Profesor directorInicial) {
        if (directorInicial == null) throw new IllegalArgumentException("Se requiere un director.");
        this.profesores = new ArrayList<>();
        this.director = directorInicial;
        this.profesores.add(directorInicial);
    }

    public void addProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor nulo.");
        profesores.add(p);
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= profesores.size()) throw new IndexOutOfBoundsException();
        if (profesores.get(pos) == director) throw new IllegalStateException("No se puede eliminar al director.");
        profesores.remove(pos); // List gestiona el reajuste automáticamente
    }
    
    // Método solicitado para devolver toda la colección protegiendo la encapsulación
    public List<Profesor> getTodosLosProfesores() {
        return Collections.unmodifiableList(profesores); 
    }
    
    // Resto de getters y setters con lógica simplificada gracias a List...
}

```
Prof: con List:
- Facilito un tamaño dinámico e ilimitado.
- Facilita el eliminado (no hay que gestionar el array), se delega en métodos de List.
---

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

La composición recursiva tiene lugar cuando una clase posee un atributo (o colección de atributos) cuyo tipo es exactamente el de la misma clase que se está definiendo. Este patrón de diseño estructural es de gran utilidad para modelar estructuras jerárquicas, redes de relaciones, o conjuntos anidados. En el caso de una entidad inmutable como `Persona`, el constructor requerirá obligatoriamente el suministro de la referencia a otro objeto de su misma clase, asumiendo un caso base nulo (como una persona sin ancestros registrados) para poder terminar la cadena de creación.

Además del árbol genealógico, en el desarrollo de software abundan los casos clásicos de estructuras basadas en composición recursiva. Entre ellos destacan los nodos de las listas enlazadas (un `Nodo` contiene una referencia al siguiente `Nodo`), los árboles binarios (un `NodoArbol` contiene referencias a sus sub-nodos hijo izquierdo y derecho), o el modelado de los sistemas de archivos (una clase `Directorio` puede contener una lista de otros objetos `Directorio` anidados).

A continuación se muestra el ejemplo en código de esta composición, implementando una clase inmutable donde el encadenamiento de objetos permite ascender por el linaje:

```java
public class Persona {
    private final String nombre;
    private final Persona madre; // Composición recursiva

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }

    public static void main(String[] args) {
        // La abuela es el "caso base", no se registra a su madre
        Persona abuela = new Persona("María", null);
        
        // La madre tiene una referencia a la abuela
        Persona madre = new Persona("Laura", abuela);
        
        // El nieto tiene una referencia a la madre
        Persona nieto = new Persona("Carlos", madre);

        System.out.println("Nieto: " + nieto.getNombre());
        System.out.println("Madre del nieto: " + nieto.getMadre().getNombre());
        System.out.println("Abuela del nieto: " + nieto.getMadre().getMadre().getNombre());
    }
}

```
Prof:
- Composiciones reflexivas o recursivas
- Composiciones bidimensionales

Las bidireccionales exigen programar cuidadosamente para mantener la consistencia. P.ej: Si añado n profesor al departamento, debo actualizar la referencia al Departamento desde Profesor.

---

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Una relación de composición (o asociación) bidireccional ocurre cuando ambos objetos implicados mantienen referencias mutuas entre sí. En lugar de que la navegación fluya en un solo sentido (por ejemplo, el departamento conoce a sus profesores, pero los profesores desconocen su departamento), en una relación bidireccional la clase `Departamento` tiene una colección de objetos `Profesor`, y, a su vez, la clase `Profesor` posee un atributo que apunta hacia su objeto `Departamento` contenedor.

Para implementar esto en Java, se requiere un diseño cuidadoso, ya que existe el riesgo de crear bucles infinitos durante la asignación del estado. Cuando se añade un `Profesor` al `Departamento`, es necesario que internamente el código se encargue de actualizar la referencia del profesor para que apunte a ese mismo departamento. Inversamente, si un `Profesor` se inicializa pasándole su `Departamento`, el código debe asegurar que el profesor quede insertado en la lista interna de dicho departamento.

Habitualmente, este desafío se resuelve designando a una de las dos clases como la responsable de mantener la coherencia de la relación mutua. Por ejemplo, se puede establecer un método público solo en el `Departamento` (como `contratarProfesor`), y hacer que los métodos del `Profesor` encargados de asignar el departamento tengan visibilidad de "paquete" (package-private), garantizando así que la doble vinculación se gestione siempre de forma simultánea y segura desde un único lugar controlado.