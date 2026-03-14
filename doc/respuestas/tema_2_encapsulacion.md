+<!--
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

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

La **encapsulación** es el mecanismo de agrupar datos y métodos en una unidad (la clase), mientras que la **ocultación de información** significa que se exponen solo las operaciones necesarias al exterior, ocultando los detalles internos. Esto es una extensión natural de la programación modular en C: si en C se ocultaba la implementación interna de un módulo mediante funciones estáticas y headers bien diseñados, en OOP se fuerza esta práctica a nivel de lenguaje mediante modificadores de acceso.

Las ventajas de la ocultación incluyen: (1) **Flexibilidad interna**: Se puede cambiar la implementación interna sin afectar al código cliente, siempre que la interfaz pública se mantenga. Por ejemplo, cambiar cómo se almacenan los datos internamente sin que el usuario lo note. (2) **Prevención de usos incorrectos**: Los atributos privados no pueden ser modificados arbitrariamente, evitando que el objeto quede en estados inválidos. (3) **Mantenibilidad**: Es más fácil entender y depurar si solo se conoce la interfaz pública. (4) **Reutilización de código**: Las clases bien encapsuladas son más fáciles de reutilizar en otros proyectos.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La **interfaz pública** de una clase es el conjunto de métodos y atributos marcados como `public` que el mundo exterior puede usar. Es el contrato que la clase ofrece: promete que estos métodos funcionarán de cierta forma y aceptarán ciertos parámetros. La interfaz pública define cómo se interactúa con la clase, similar a cómo en C se definen funciones públicas en un header `.h` que otros archivos pueden usar.

La relación con la ocultación es directa: todo lo que no está en la interfaz pública (métodos privados, atributos privados) está oculto. Esto permite que el diseñador de la clase tenga libertad total para cambiar lo privado sin romper el código cliente, siempre que se respete lo público. Por ejemplo, una clase `Punto` podría ocultar cómo calcula distancias (privadamente), mientras expone públicamente un método `calcularDistanciaAOrigen()`. Si internamente decides cambiar el algoritmo de cálculo, el usuario de la clase no se entera porque solo ve el método público.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

La interfaz pública debe diseñarse con cuidado porque una vez que se publica, **es muy difícil cambiarla**. Cambiar la interfaz pública significa que todo código cliente que dependa de ella se rompe. Si una clase `Calculadora` expone públicamente un método `sumar(int a, int b)`, y después decides que debe ser `sumar(double a, double b)`, todos los clientes tendrán que recompilar y ajustar sus llamadas. Esto es similar a en C: si cambias la firma de una función en un header, todos los archivos que la usan necesitan recompilación.

Por eso es importante pensar a largo plazo en la interfaz pública: ¿Qué operaciones realmente necesita exponer la clase? ¿Hay métodos que solo son auxiliares internos? Una buena práctica es mantener la interfaz pública **mínima y estable**, marcando como privados todos los detalles de implementación. Cambiar lo privado es seguro; cambiar lo público requiere una nueva versión de la clase.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Las **invariantes de clase** son condiciones que deben mantenerse verdaderas en todo momento para que el objeto sea válido. Por ejemplo, en una clase `CuentaBancaria`, una invariante sería "el saldo nunca puede ser negativo". En la clase `Punto` dentro de un círculo, una invariante sería "la distancia al origen debe ser menor que el radio del círculo". Las invariantes representan las reglas del negocio o la lógica del problema que la clase modela.

La ocultación de información es crucial para mantener invariantes porque: Si el atributo `saldo` fuera público, cualquier código podría hacer `cuenta.saldo = -1000;` y violaría la invariante. Marcándolo privado y proporcionando un método `retirar(double monto)` que valida antes de cambiar el saldo, se garantiza que el invariante se mantiene. En C, esto era imposible de garantizar: tú podías hacer `cuenta.saldo = -1000;` accediendo directamente a la estructura. La ocultación permite que el diseñador de la clase sea el responsable exclusivo de mantener las invariantes, delegando al compilador el trabajo de impedir violaciones.



## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

```java
public class Punto {
    // Atributos privados: ocultos al mundo exterior
    private double x, y;
    
    // Constructor público: la única forma de crear un Punto
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    // Método público: permite calcular distancia al origen
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
    
    // Getters públicos: permiten leer (no modificar) los atributos
    public double getX() { return x; }
    public double getY() { return y; }
}
```

La **interfaz pública** de esta clase consiste en: el constructor `Punto(double, double)`, el método `calcularDistanciaAOrigen()`, y los getters `getX()` y `getY()`. **`public`** significa que cualquier código puede acceder a eso (métodos y atributos públicos). **`private`** significa que solo el código dentro de la clase puede acceder (atributos privados, métodos auxiliares). En C, simularías esto con funciones declaradas como `static` en un archivo .c (privadas) e incluidas en el .h (públicas), pero Java lo fuerza a través del lenguaje.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

Los modificadores `public` y `private` pueden aplicarse a: (1) **Atributos (variables miembro)**: Como `private double x;` o `public static final double PI;`. (2) **Métodos**: Como `public void mover(int dx)` o `private void validar()`. (3) **Constructores**: Como `public Punto(double x, double y)` o `private Punto()`. (4) **Clases anidadas (inner classes)**: Una clase puede contener otra clase, que también puede ser pública o privada.

Algo que **NO** puede llevar estos modificadores: (1) Las **variables locales** dentro de métodos siempre son privadas implícitamente (no se puede escribir `private int x = 5;` dentro de un método). (2) En versiones antiguas de Java, los modificadores no se aplicaban a las clases de nivel superior (solo `public` o paquete-privada), pero con las características modernas, sí puedes tener clases privadas en ciertos contextos.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

Sí, existen más niveles de visibilidad además de público y privado. En Java, hay cuatro niveles: (1) **`public`**: Accesible desde cualquier parte del programa. (2) **`private`**: Accesible solo dentro de la clase. (3) **`protected`**: Accesible dentro de la clase, en clases del mismo paquete (package), y en subclases. (4) **Sin modificador (package-private)**: Accesible solo dentro del mismo paquete. El modificador `protected` está relacionado con la herencia, que verás en temas posteriores.

Otros lenguajes tienen variaciones: **C++** tiene `public`, `private`, `protected` (similar a Java), pero sin el concepto de paquete. **C#** tiene `public`, `private`, `protected`, `internal` (similar a package-private), `protected internal`, y `private protected`. **Python** no tiene modificadores formales (todo es técnicamente público), pero usa la convención de prefijo `_` para privado y `__` para muy privado. Una regla general es: Java proporciona un balance entre control fino y simplicidad.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

La respuesta es **(a) otras clases**. Los miembros privados están ocultos solo para otras clases, **NO** para otras instancias de la misma clase. Esto significa que dentro de la clase, una instancia puede acceder a los miembros privados de otra instancia de la misma clase.

```java
public class Punto {
    private double x, y;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    // Este método recibe OTRO Punto y puede acceder a sus atributos privados
    public double calcularDistanciaAPunto(Punto otro) {
        // otro.x y otro.y son privados, pero puedo accederlos porque
        // estoy dentro de la clase Punto
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Esta decisión tiene sentido: una instancia de `Punto` confía en otra instancia de `Punto`. El encapsulamiento protege contra código **externo** (otras clases), no contra instancias hermanas. La alternativa (ocultar entre instancias) sería tan restrictiva que sería impráctica: no podrías comparar dos objetos sin getters públicos. En C++, esto se expresa con `friend`, pero en Java es simplemente el comportamiento estándar de `private`.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta

Los **getters** (métodos de obtención) son métodos públicos que devuelven el valor de un atributo privado, permitiendo al usuario **leer** (pero no modificar) el estado del objeto. Un getter típicamente tiene la forma `public <tipo> get<NombreAtributo>()`, como `public double getX()`. Los **setters** (métodos de asignación) son métodos públicos que permiten cambiar el valor de un atributo privado desde afuera, pero con potencial de validación. Un setter típicamente tiene la forma `public void set<NombreAtributo>(<tipo> valor)`, como `public void setX(double x) { this.x = x; }`

La utilidad es: Si un atributo fuera público, cualquiera podría asignarlo a cualquier valor sin restricciones. Con getters/setters, el diseñador de la clase puede validar: por ejemplo, un setter de edad podría rechazar valores negativos. En C, esto sería `int getEdad(Persona *p) { return p->edad; }` y `void setEdad(Persona *p, int edad) { if (edad >= 0) p->edad = edad; }`, pero Java lo formaliza con la convención de nombres. Una alternativa moderna es usar **records** en Java 14+ que generan getters automáticamente, pero setters deben ser explícitos.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

No, la "seguridad" aquí **no significa seguridad cibernética** contra ataques. Se refiere a **seguridad lógica**: que el programa funcione correctamente y no entre en estados inválidos por culpa de usos incorrectos. La ocultación de información previene que el programador cometa errores accidentales: si declares `x` privado, no puedes escribir accidentalmente `punto.x = Double.NaN;` desde afuera, violando la integridad del objeto.

Desde una perspectiva de seguridad cibernética, la ocultación de información en Java **no ofrece protección real** porque el bytecode puede ser decompilado y los accesos a privado pueden ser burlados con reflection. Si necesitas verdadera seguridad cibernética, necesitas otras técnicas (criptografía, validación del lado del servidor, etc.). La ocultación de información es una herramienta de **correctness de software** (que el programa haga lo correcto) y **mantenibilidad** (que sea fácil de cambiar sin romper cosas), no de seguridad en sentido cibernético.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta

Un **miembro de instancia** (atributo o método) tiene un valor distinto para cada instancia de la clase. Por ejemplo, en `Punto`, `x` e `y` son miembros de instancia: cada punto tiene sus propias coordenadas. Un **miembro de clase** (atributo o método) pertenece a la clase en su conjunto, no a instancias individuales, y es compartido por todas las instancias. Se declara con la palabra clave `static`. Por ejemplo, `static int contadorPuntos;` sería un miembro de clase que cuenta cuántos puntos se han creado: todas las instancias comparten este contador. En C, esto sería equivalente a una variable global dentro de un módulo.

Los miembros de clase **también se pueden ocultar** con `private static`. Por ejemplo, `private static int contadorPuntos;` es inaccesible desde afuera, aunque podrías proporcionar un método público `public static int getContadorPuntos()` para leerlo. Esto es útil para mantener estado privado de la clase: información que la clase necesita internamente pero que no quiere exponer. La regla es la misma: `public` es accesible desde cualquier lugar, `private` solo desde dentro de la clase, sin importar si es miembro de instancia o de clase.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí, los constructores privados tienen sentido en ciertos casos. Uno de los más comunes es el **patrón Singleton**: una clase que solo debería tener una instancia. Si el constructor es privado, nadie puede hacer `new` para crear más instancias. La clase expone un único punto de acceso, como `public static Singleton getInstance()`, que devuelve la única instancia. Otro caso es cuando quieres **forzar el uso de métodos factoría**: en lugar de permitir que cualquiera llame al constructor, lo haces privado y proporcionas métodos `static` que crean instancias de formas específicas. Por ejemplo, una clase `Punto` podría tener un constructor privado y dos métodos factorías: `public static Punto desdeCoordinadas(double x, double y)` y `public static Punto desdePolar(double r, double theta)`.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

Los **miembros de clase** en Java se indican con la palabra clave **`static`**. Esto significa que existen una sola copia para toda la clase, no una por instancia.

```java
public class Punto {
    // Miembros de instancia
    private double x, y;
    
    // Miembros de clase (static): compartidos por todas las instancias
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        // Actualizar máximos globales
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }
    
    // Métodos de clase (static) para acceder a los máximos
    public static double getMaxX() {
        return maxX;
    }
    
    public static double getMaxY() {
        return maxY;
    }
}
```

Nota que `getMaxX()` y `getMaxY()` también son `static`: los métodos que acceden a miembros de clase suelen ser estáticos. Se invocan así: `Punto.getMaxX()`, no `punto.getMaxX()` (aunque Java permite ambas). En C, esto sería equivalente a variables globales dentro del módulo, pero menos error-prone porque están encapsuladas dentro de la clase.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta

```java
public static Punto desdeCoordinadasRedondeadas(double x, double y) {
    double xRedondeada = Math.round(x);
    double yRedondeada = Math.round(y);
    return new Punto(xRedondeada, yRedondeada);
}
```

Sí, se ha usado `static`. Este es un **método factoría**: un método estático que actúa como alternativa al constructor directo. Se pueden crear puntos así: `Punto p = Punto.desdeCoordinadasRedondeadas(3.7, 4.2);`, en lugar de tener que redondear manualmente antes de llamar al constructor. El método factoría encapsula la lógica de creación con validación/transformación. Los métodos factoría son especialmente útiles cuando necesitas crear objetos de múltiples formas: podrías tener `desdeCartesiano(x, y)`, `desdePolar(r, theta)`, `desdeCoordinadasRedondeadas(x, y)`, etc. Implementación típica: el constructor podría ser privado, forzando a los usuarios a usar los métodos factoría.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

```java
public class Punto {
    // Implementación privada: array interno
    private double[] coordenadas = new double[2];
    
    public Punto(double x, double y) {
        coordenadas[0] = x;
        coordenadas[1] = y;
    }
    
    public double getX() {
        return coordenadas[0];
    }
    
    public double getY() {
        return coordenadas[1];
    }
    
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] + 
                           coordenadas[1] * coordenadas[1]);
    }
}
```

Note que la interfaz pública sigue siendo la misma: constructor, getters, método de cálculo. El código cliente no nota el cambio. Esta es la esencia del encapsulamiento: **la implementación interna es flexible, la interfaz pública es estable**. Cambiar de dos variables individuales a un array internamente es invisible desde afuera. En C, conseguir esto requeriría separar completamente la definición (en .h) de la implementación (en .c) y usar funciones para acceder, lo cual no se fuerza a nivel de lenguaje.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

No, es mejor mantenerlo privado incluso con getter/setter públicos porque el setter te permite **validar y controlar** cómo cambia el atributo. Por ejemplo: `public void setEdad(int edad) { if (edad < 0) throw new IllegalArgumentException("Edad no puede ser negativa"); this.edad = edad; }`. Si la edad fuera pública, se podría escribir `persona.edad = -5;` directamente, violando la lógica del negocio. Además, si más adelante necesitas ejecutar código cuando un atributo cambia (logging, notificación de cambios, recálculos), el setter es el lugar para hacerlo sin cambiar la interfaz pública.

La **convención es: atributos privados, acceso mediante getter/setter públicos** (o alternativamente, propiedades en lenguajes que las soportan como C#). Esto está directamente relacionado con **invariantes de clase**: el setter es responsable de mantener los invariantes. En el ejemplo anterior, el invariante es "edad >= 0", y el setter lo enforce. Si el atributo fuera público, no habría forma de garantizar el invariante. Esta es una diferencia fundamental respecto a estructuras simples en C: con structs, los invariantes eran responsabilidad del programador (confiar en que no hiciera cosas tontas); con clases bien encapsuladas, los invariantes son responsabilidad de la clase.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase **inmutable** es aquella cuyo estado no puede cambiar después de su creación. Una vez creado el objeto, sus atributos son constantes. La clase de ejemplo clásica es `String` en Java. Para hacer una clase inmutable: (1) todos los atributos deben ser `private final`, (2) no debe haber setters, (3) el constructor inicializa todos los atributos, (4) si la clase tiene atributos mutables (como arrays), no debe exponer referencias directas (para evitar cambios externos).

Un **método modificador** es cualquier método que cambia el estado del objeto. Los setters son métodos modificadores, pero también pueden serlo otros: `punto.trasladar(dx, dy)` modifica las coordenadas del punto sin ser un setter. Un método modificador **no siempre es un setter**: un setter solo cambia un atributo; un modificador puede cambiar varios o realizar lógica compleja. Las **ventajas de la inmutabilidad** incluyen: (1) **Thread-safety**: Los objetos inmutables son seguros para usarse desde múltiples hilos sin sincronización. (2) **Predictibilidad**: El estado de un objeto nunca sorprende; siempre es el mismo. (3) **Cacheabilidad**: Se pueden reutilizar instancias sin preocuparse por cambios (Java interna cachea strings). (4) **Simplicidad**: Menos bugs relacionados con cambios inesperados de estado.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

**No**, definir setters para todos los atributos es una mala práctica. Los setters deben incluirse solo cuando tiene sentido que el atributo cambie después de la creación del objeto. En una clase `Persona`, `setEdad()` tiene sentido porque la edad cambia; pero en una clase `DocumentoIdentidad`, el número de ID y la fecha de emisión no deberían cambiar nunca, así que no hace sentido tener setters públicos. La regla es: **si un atributo debe ser mutable, proporciona un setter; si debe ser inmutable, no lo hagas**.

Adicionalmente, a menudo es mejor que los setters no sean simples adaptadores a atributos privados, sino que realicen **validaciones y lógica de negocio**. Por ejemplo, en lugar de un setter genérico `setSalario(double s)`, podrías tener `darAumento(double porcentaje)` que valida que el porcentaje sea positivo y actualiza el salario de manera controlada. Esto refuerza los invariantes: el setter es el guardián del estado válido. En conclusión: menos setters, pero mejores; métodos modificadores que reflejan las operaciones del negocio, no simples mutadores de atributos.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

La clase `String` en Java es **inmutable**: una vez creada, no puede cambiar. Esto significa que no tiene setters y sus atributos internos son `final`. Al concatenar dos cadenas con `s1 + s2`, no se modifica `s1` ni `s2`. En su lugar, se crea una **nueva instancia de String** que contiene el resultado. Por ejemplo: `String s = "Hola" + " " + "Mundo";` crea internamente tres strings nuevos ("Hola", " ", "Mundo") y dos strings intermedios ("Hola ", "Hola Mundo"), siendo el último el devuelto. En C, en cambio, `strcat()` modifica la cadena original in-place.

Cuando necesitas concatenar muchas veces en un bucle, **debes usar `StringBuilder`** (o `StringBuffer` si es thread-safe): `StringBuilder sb = new StringBuilder(); for(...) { sb.append(dato); } String resultado = sb.toString();`. `StringBuilder` es mutable internamente, por lo que las concatenaciones sucesivas son eficientes (solo reasigna cuando es necesario). Si usaras `String` directamente en un bucle, crear nuevas instancias cada vez sería muy lento. Esto es una demostración de que la inmutabilidad de String es un trade-off: es segura y predecible, pero requiere herramientas especiales para operaciones frecuentes de modificación.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta

En POO, la comparación de objetos puede significar dos cosas: (1) **Comparación por identidad**: ¿son el mismo objeto? (mismo lugar en memoria). Se usa el operador `==`, que compara referencias. (2) **Comparación por contenido (valor)**: ¿tienen el mismo estado interno?. Se usa el método `equals()`. El **método `equals()`** es un método especial de la clase `Object` (clase base de todas las clases) que permite comparar por contenido. Por defecto, `equals()` hace lo mismo que `==` (compara identidad). Para una comparación significativa por valor, la clase debe sobrescribir `equals()`.

Por ejemplo, sin sobrescribir: `new String("Hola").equals(new String("Hola"))` sería `false` por defecto (diferente identidad). Pero `String` sobrescribe `equals()` para comparar contenido, así que devuelve `true`. **Nunca uses `==` para comparar cadenas en Java; siempre usa `equals()` o `compareTo()`**. Para objetos propios, sobrescribe `equals()` si tiene sentido comparar por valor. Es importante notar: si sobrescribes `equals()`, generalmente deberías sobrescribir también `hashCode()` para mantener la consistencia (si dos objetos son iguales según `equals()`, deben tener el mismo hash).


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta

Las **clases wrapper** son clases que envuelven tipos primitivos para darles la apariencia de ser objetos. En Java, existen para cada tipo primitivo: `Integer` envuelve `int`, `Double` envuelve `double`, `Boolean` envuelve `boolean`, etc. Se crean explícitamente así: `Integer num = new Integer(5);` o `Integer num = Integer.valueOf(5);`. A partir de Java 5, existe **autoboxing**: el compilador convierte automáticamente `int` a `Integer` y viceversa, así que puedes escribir `Integer num = 5;` (boxing) o `int x = num;` (unboxing) sin llamadas explícitas. El compilador inserta las conversiones automáticamente.

Las ventajas de los wrappers incluyen: (1) **Uniformidad**: A veces necesitas tratar primitivos como objetos (por ejemplo, en colecciones genéricas como `List<Integer>`). (2) **Métodos útiles**: `Integer` proporciona métodos como `parseInt()`, `toString()`, `compare()`. (3) **Null**: Un objeto wrapper puede ser `null`, lo cual no es posible con primitivos. Las desventajas son: (1) **Rendimiento**: Los wrappers toman más memoria y tienen overhead. (2) **Complejidad**: Autoboxing puede causar sorpresas si no se entiende bien. No todos los lenguajes OOP tienen tipos primitivos: **Python** no tiene primitivos (todo es objeto). **C#** tiene los dos (tipos primitivos `int` y clase wrapper `Int32`). **Ruby** y **Smalltalk** también tienen todo como objeto.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clas? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un **tipo de dato enumerado** es un tipo que define un conjunto cerrado y fijo de valores posibles. Por ejemplo, `Color` podría ser una enumeración con valores `ROJO`, `VERDE`, `AZUL`. En lugar de usar strings o ints para representar colores (proclives a errores), usas valores del enum, que son type-safe. En Java, un enum es de hecho **una clase especial** (hereda de la clase `Enum`). Se declara así:

```java
public enum Color {
    ROJO, VERDE, AZUL
}
```

Las ventajas en términos de encapsulación son: (1) **Type-safety**: El compilador ensure que solo son válidos los valores del enum. No puedes pasar un string incorrecta como color; tienes que usar `Color.ROJO`. (2) **Validación automática**: No hay valores inválidos por accidente. En C, usarías integer constants `#define ROJO 0`, y nada te impide hacer `color = 999;`. (3) **Métodos en enums**: Puedes agregar métodos y campos al enum (como verás en la pregunta siguiente), manteniéndolos encapsulados. (4) **Iterabilidad**: Java proporciona métodos como `values()` para iterar sobre todas las opciones. Los enums favorecen la escritura de código más seguro y expresivo.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta

```java
public enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);
    
    private final int dias;      // Atributo privado
    private final int ordinal;   // Atributo privado
    
    // Constructor del enum (implícitamente privado)
    Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }
    
    public int getDias() {
        return dias;
    }
    
    public int getOrdinal() {
        return ordinal;
    }
    
    public boolean esDePrimavera(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == MARZO || this == ABRIL || this == MAYO;
        } else {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        }
    }
    
    public boolean esDeVerano(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        } else {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        }
    }
    
    public boolean esDeOtoño(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        } else {
            return this == MARZO || this == ABRIL || this == MAYO;
        }
    }
    
    public boolean esDeInvierno(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        } else {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        }
    }
}
```

Uso: `Mes m = Mes.ENERO; System.out.println(m.getDias()); System.out.println(m.esDePrimavera(true));`. Este ejemplo demuestra cómo los enums pueden tener atributos privados, constructores, y métodos. El enum encapsula completamente la información de cada mes, incluyendo la lógica de estaciones. Es mucho más seguro que usar integers (0-11) o strings para representar meses.
