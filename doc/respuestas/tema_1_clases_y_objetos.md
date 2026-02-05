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

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

La **Encapsulación** es el mecanismo de agrupar datos (atributos) y operaciones (métodos) dentro de una unidad, ocultando los detalles internos al exterior. Esto es similar a como en C podría agruparse una estructura con funciones que operan sobre ella, pero en OOP el acceso se controla mediante niveles de visibilidad (público, privado, etc.). La **Abstracción** permite representar objetos del mundo real simplificados en el código, exponiendo solo lo esencial y ocultando la complejidad innecesaria.

La **Herencia** permite que una clase derive de otra, reutilizando sus atributos y métodos. A diferencia de C, donde se tendría que duplicar código o usar funciones genéricas, la herencia proporciona una forma elegante de crear jerarquías. El **Polimorfismo** ("muchas formas") permite que un mismo método tenga diferentes comportamientos según el tipo de objeto. Esto es conceptualmente diferente al C tradicional, donde una función siempre hace lo mismo, pero es similar a tener punteros a funciones que apuntan a diferentes implementaciones.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

Algunos de los lenguajes más populares que soportan programación orientada a objetos son: **Java** (usado ampliamente en aplicaciones empresariales), **C++** (extensión orientada a objetos de C, que proporciona herencia y polimorfismo), **Python** (lenguaje versátil con soporto total para OOP), **C#** (lenguaje de Microsoft con características muy similares a Java), **JavaScript** (lenguaje de scripting que en sus versiones modernas tiene clases), y **Kotlin** (que se ejecuta en la máquina virtual de Java).


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

La **programación estructurada** surgió a finales de los años 60 como respuesta a los problemas del código desorganizado ("goto spaghetti code"). Se basa en el uso de estructuras de control como bucles (for, while) y condicionales (if-else) en lugar de saltos incondicionales. Utiliza funciones como bloques organizadores básicos, permitiendo dividir el problema en subproblemas manejables. Este paradigma es el que probablemente has aprendido con C/C++: escribir funciones que hacen una tarea específica.

La **programación modular** va un paso más allá: organiza el código en módulos independientes, cada uno con una responsabilidad clara. Un módulo en C sería un archivo .c con un .h asociado, que exporta ciertas funciones públicas mientras mantiene privadas otras (mediante `static`). Sin embargo, la modulación en programación estructurada es voluntaria y frágil: nada previene que se acceda directamente a variables globales. La POO automatiza esta idea, haciendo que el encapsulamiento sea fundamental en la estructura del lenguaje.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

Los tres elementos que definen un objeto son: **identidad** (quién es), **atributos o estado** (qué tiene) y **comportamiento o métodos** (qué hace). La **identidad** es lo que diferencia este objeto de otros en memoria: en C, sería la dirección de memoria de una estructura, y en Java, cada objeto tiene una dirección única asignada por la máquina virtual. Los **atributos** son variables que almacenan el estado del objeto (como x e y en un Punto), y son equivalentes a los miembros de un struct en C. El **comportamiento** corresponde a las operaciones que el objeto puede realizar, implementadas como métodos (funciones) que operan sobre sus atributos, accediendo a ellos sin necesidad de pasar la estructura como parámetro explícitamente.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta

Una **clase** es una plantilla o molde que define qué atributos y métodos tienen los objetos de ese tipo. Es comparable a una estructura en C (`struct Punto { int x; int y; }`), pero además de datos, la clase especifica las operaciones (métodos) disponibles. Un **objeto** es una instancia concreta de una clase: es como una variable de tipo struct en C, pero vivo en memoria con su identidad propia. Una **instancia** es otro término para referirse a un objeto: es la manifestación concreta de una clase. Existe una clara distinción entre la clase (el plano) y el objeto (la cosa construida según el plano).

No todos los lenguajes orientados a objetos manejan clases explícitamente. Algunos, como **JavaScript** y **Lua**, utilizan un modelo basado en **prototipos**, donde los objetos se crean directamente sin necesidad de una clase predefinida. Sin embargo, la mayoría de los lenguajes principales (Java, C++, C#, Python) sí utilizan el modelo de clases, que proporciona más estructura y seguridad de tipos.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta

En Java, los objetos se almacenan en el **heap** (montículo), que es una región de memoria dinámica. Cuando se crea un objeto con `new`, se reserva espacio en el heap y se devuelve una referencia (similar a un puntero en C) que se guarda en una variable local o en el stack. Esto es diferente a los lenguajes compilados como C++, donde los objetos pueden estar en el stack (si son variables locales) o en el heap (si se crean con `new`/`delete`). La decisión depende del lenguaje y sus convenciones.

La **recolección de basura** (garbage collection) es un mecanismo automático que libera la memoria ocupada por objetos que ya no se usan. La máquina virtual de Java mantiene referencias a todos los objetos vivos y periódicamente elimina aquellos que no son alcanzables desde ningún punto del programa. En C/C++, el programador debe liberar manualmente la memoria con `delete`, lo que es propenso a errores (memory leaks o use-after-free). La recolección automática de Java elimina estos problemas, aunque tiene un costo en rendimiento y no está disponible en todos los lenguajes: C++ no tiene garbage collection automático, aunque versiones modernas ofrecen smart pointers.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta

Un **método** es una función que pertenece a una clase y opera sobre los atributos de un objeto. Es conceptualmente similar a una función en C que recibe un puntero a una estructura, pero sin necesidad de pasarlo explícitamente: dentro del método, se accede directamente a los atributos del objeto mediante `this`. Cada vez que se invoca un método sobre un objeto, automáticamente se le pasa la referencia del objeto receptor.

La **sobrecarga de métodos** permite definir múltiples métodos con el mismo nombre pero diferentes parámetros (diferente tipo, número o ambos). Por ejemplo, una clase Calculadora podría tener un método `suma` que reciba dos enteros y otro que reciba dos decimales. El compilador determina automáticamente cuál llamar basándose en los argumentos proporcionados. En C, esto no es posible: se tendrían que nombrar las funciones de forma diferente (`suma_int`, `suma_float`). La sobrecarga es un ejemplo del polimorfismo ad-hoc: el mismo nombre, diferentes comportamientos según el contexto.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta

```java
public class Punto {
    double x, y;  // Atributos con visibilidad por defecto (package-private)
    
    public double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

Para usar esta clase:

```java
public class Main {
    public static void main(String[] args) {
        Punto p = new Punto();  // Crear una instancia
        p.x = 3.0;              // Asignar valores a los atributos
        p.y = 4.0;
        
        double distancia = p.calculaDistanciaAOrigen();  // Llamar al método
        System.out.println("Distancia: " + distancia);   // Imprime: Distancia: 5.0
    }
}
```


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

El punto de entrada en un programa Java es el método `main` con firma exacta: `public static void main(String[] args)`. Este método se ejecuta automáticamente cuando se corre la aplicación. La palabra clave **`static`** indica que el método pertenece a la clase, no a una instancia particular de la clase. Por tanto, puede invocarse sin crear un objeto: `java Main` ejecuta `Main.main()` directamente. En C, todas las funciones son implícitamente "estáticas" en este sentido, pero en Java la mayoría de métodos requieren un objeto.

**`static`** se usa también para atributos (variables de clase compartidas por todas las instancias) y otros métodos auxiliares que no necesitan estado de instancia. Por ejemplo, `Math.sqrt()` es estático porque la función raíz cuadrada no necesita operar sobre un objeto Math específico. Cuando **`static` se combina con `final`**, se crea una constante de clase: `public static final double PI = 3.14159;`. Esto significa que la variable es estática (pertenece a la clase), no puede cambiar (`final`), y típicamente se usa para valores constantes que toda la aplicación comparte. En C, esto sería equivalente a `const double PI = 3.14159;` global, pero con mejor encapsulamiento.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta

Para compilar y ejecutar un programa Java desde línea de comandos:

```bash
javac Punto.java          # Compila el archivo .java a bytecode
java Punto                # Ejecuta la clase compilada
```

Java es **compilado a un nivel intermedio**. A diferencia de C, que se compila a código máquina (binario nativo), Java se compila a **bytecode**, un código intermedio generado por el compilador `javac` y guardado en ficheros `.class`. El bytecode no es código máquina nativo, sino instrucciones para la **máquina virtual de Java (JVM)**. La JVM es un programa que interpreta/ejecuta este bytecode en tiempo de ejecución. Esto aporta portabilidad: el mismo `.class` funciona en cualquier máquina que tenga instalada una JVM, sin recompilar.

Esta arquitectura de dos etapas (compilación a bytecode + ejecución en JVM) es la razón principal de la portabilidad de Java ("write once, run anywhere"). El compilador detecta errores de tipos y sintaxis, mientras que la JVM proporciona servicios de runtime como la recolección de basura, manejo de excepciones y compilación JIT (just-in-time) a código nativo para mejorar rendimiento. En C/C++, la compilación produce directamente código máquina nativo, lo que es más rápido pero menos portable: debe recompilarse para cada arquitectura.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta

La palabra clave **`new`** realiza tres pasos: (1) asigna memoria en el heap para el nuevo objeto, (2) invoca el **constructor** de la clase para inicializar el objeto, y (3) devuelve una referencia al objeto creado. En C, esto sería similar a hacer `malloc()` seguido de una función de inicialización, pero `new` lo hace todo automáticamente. Un **constructor** es un método especial que se llama automáticamente cuando se crea un objeto. Tiene el mismo nombre que la clase, no retorna nada (ni siquiera `void`), e inicializa los atributos del objeto a valores válidos.

```java
public class Empleado {
    String dni, nombre, apellidos;
    
    // Constructor
    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

Uso:

```java
Empleado emp = new Empleado("12345678A", "Juan", "García López");
```

Los constructores son útiles porque garantizan que cada objeto se crea en un estado válido. Si no se define un constructor explícitamente, Java crea automáticamente uno vacío (constructor por defecto) que no hace nada. En C++, los constructores son muy similares; en C hay que hacerlo manualmente con funciones de inicialización como `inicializar_empleado(&emp, dni, nombre, apellidos)`.


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta

**`this`** es una referencia implícita al objeto actual sobre el cual se invoca un método. Es como si cada método recibiera automáticamente un parámetro oculto que apunta al objeto: cuando se escribe `this.x`, se accede al atributo `x` del objeto actual. En C, se tendría que pasar explícitamente: `void calculaDistancia(Punto *this)` y luego escribir `this->x`. La utilidad de `this` es desenmascarar estas operaciones implícitas y hacer llamadas recursivas (un método que invoca a otro método del mismo objeto).

```java
public class Punto {
    double x, y;
    
    public Punto(double x, double y) {
        this.x = x;      // this.x es el atributo, x es el parámetro
        this.y = y;      // this.y es el atributo, y es el parámetro
    }
    
    public void trasladar(double dx, double dy) {
        this.x += dx;    // Modifica el atributo del objeto actual
        this.y += dy;
    }
}
```

No todos los lenguajes usan el nombre `this`: Python usa `self`, C++ usa `this` (como Java), y algunos lenguajes, como early versiones de Smalltalk, usan `super` o no tienen una palabra clave explícita.


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta

```java
public class Punto {
    double x, y;
    
    public double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
    
    public double distanciaA(Punto otro) {
        double dx = this.x - otro.x;  // Diferencia en x
        double dy = this.y - otro.y;  // Diferencia en y
        return Math.sqrt(dx * dx + dy * dy);  // Fórmula de distancia
    }
}
```

Uso:

```java
Punto p1 = new Punto(3, 4);
Punto p2 = new Punto(6, 8);
double dist = p1.distanciaA(p2);  // Distancia entre p1 y p2
```

Este método ilustra cómo acceder a los atributos de dos objetos: el objeto actual (`this.x`, `this.y`) y el objeto parámetro (`otro.x`, `otro.y`). En C, se tendría que escribir `double distanciaA(Punto *this, Punto *otro)` y acceder mediante `this->x`, `otro->x`. El paso del objeto como parámetro establece un contrato: se dice qué información necesita el método para completar su tarea.


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta

En Java, el paso de objetos como `Punto` es **por referencia**: se pasa la dirección en memoria del objeto, no una copia. Por tanto, si un método modifica un atributo del objeto parámetro, esos cambios persisten fuera del método. Esto es importante porque en Java todas las variables que guardan objetos son referencias, no los objetos en sí.

```java
public void trasladarPunto(Punto p) {
    p.x = 100;  // Modifica el objeto original, no una copia
    p.y = 200;
}

// Uso:
Punto p = new Punto(1, 1);
trasladarPunto(p);
System.out.println(p.x);  // Imprime 100
```

En contraste, cuando se pasan tipos primitivos como `int`, el paso es **por valor**: se copia el valor, y las modificaciones dentro del método no afectan al original. Esto es coherente con C: en C, pasar `int x` es por valor (como en Java), pero pasar un puntero `int *x` es por referencia (similar a pasar un objeto en Java). La regla simple es: en Java, los objetos siempre se pasan por referencia, y los tipos primitivos (`int`, `double`, `boolean`) siempre por valor.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta

**`toString()`** es un método especial de la clase `Object` (clase base de todas las clases en Java) que devuelve una representación textual del objeto. Java lo invoca automáticamente cuando se intenta convertir un objeto a cadena de texto, por ejemplo, en `System.out.println(objeto)` o al concatenarlo con strings. El método por defecto devuelve algo como `Punto@a4c2f6` (nombre de clase y código hash), pero es más útil sobrescribirlo para proporcionar una representación legible.

```java
public class Punto {
    double x, y;
    
    @Override  // Anotación que indica que estamos sobrescribiendo un método
    public String toString() {
        return "Punto (" + x + ", " + y + ")";
    }
}

// Uso:
Punto p = new Punto(3, 4);
System.out.println(p);  // Imprime: Punto (3.0, 4.0)
```

Este patrón existe en muchos lenguajes: C++ tiene `operator<<`, Python tiene `__str__()`, C# tiene `ToString()`. La idea es proporcionar una forma estándar de convertir objetos a texto para debugging y logging. En C, habría que hacer una función separada como `void imprimirPunto(Punto *p)`, pero en OOP es preferible que cada clase sepa cómo representarse a sí misma.


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

### Respuesta

Una clase en Java es **conceptualmente similar** a un `struct` en C en que ambas agrupan datos (miembros/atributos). Sin embargo, hay diferencias fundamentales. Un `struct` en C es solo un contenedor de datos sin comportamiento integrado; las funciones que operan sobre ese struct están separadas. Una clase integra datos y comportamiento: los métodos pertenecen a la clase y pueden acceder directamente a los datos mediante `this`.

Para que un `struct` de C fuera como una clase de Java, necesitaría: (1) **Encapsulamiento**: control de acceso (público/privado) para los miembros, lo cual no existe en C (aunque se puede simular con convenciones); (2) **Métodos**: funciones que pertenecen al struct, lo cual no tiene equivalente directo en C; (3) **Constructores**: código que se ejecuta automáticamente al crear una instancia, en lugar de código de inicialización separado; (4) **Herencia**: capacidad de que un struct derive de otro, compartiendo datos y comportamiento. C no tiene herencia nativa (aunque puede simularse). En resumen, un struct en C es como la mitad de una clase: tiene los datos, pero carece de la encapsulación y el comportamiento integrados que caracterizan a las clases.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
```c
#include <math.h>

typedef struct {
    double x, y;
} Punto;

// La función recibe explícitamente un Punto (this)
double calculaDistanciaAOrigen(Punto *this) {
    return sqrt(this->x * this->x + this->y * this->y);
}

// Uso:
Punto p = {3.0, 4.0};
double dist = calculaDistanciaAOrigen(&p);  // Pasar explícitamente el puntero
```

Como se ve, la emulación requiere pasar explícitamente un puntero al struct (el equivalente de `this`). En Java, `this` es implícito: cuando se escribe `p.calculaDistanciaAOrigen()`, automáticamente se pasa `p` como `this`. En C, hay que hacerlo manualmente: `calculaDistanciaAOrigen(&p)`.

Esta comparación muestra la verdadera naturaleza de las clases: son structs con funciones asociadas. La principal diferencia es que Java automatiza el manejo de `this` y añade control de visibilidad. Si se organizara el código C así (una función por cada operación, todas recibiendo un puntero al struct), se tendría una simulación rudimentaria de POO. Sin embargo, falta encapsulamiento: nada impide acceder directamente a `p.x` o pasar un Punto inválido a la función. Las clases de Java fuerzan mejor práctica mediante mecanismos del lenguaje.