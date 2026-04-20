<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta

En C se puede usar un array de punteros a `void` para guardar cualquier tipo de dato, ya que `void*` es un puntero sin tipo explícito. En Java se puede usar un array de `Object`, porque todas las clases derivan de `Object` y, por tanto, cualquier referencia puede almacenarse allí.

```c
void* tabla[10];
int x = 5;
char c = 'A';
const char* texto = "hola";
tabla[0] = &x;
tabla[1] = &c;
tabla[2] = (void*) texto;
```

```java
Object[] tabla = new Object[3];
tabla[0] = 5;
tabla[1] = "hola";
tabla[2] = new Date();
```

En ambos casos se utiliza un array primitivo que admite referencias de cualquier tipo. Esa estructura permite almacenar distintos tipos en una única colección, aunque no ofrece chequeo de tipo estricto en tiempo de compilación.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica?

### Respuesta

La programación genérica permite definir estructuras y algoritmos que funcionan con tipos de datos independientes, sin fijar un tipo concreto. Esto se logra mediante parámetros de tipo, que permiten reutilizar el mismo código para distintos tipos mientras el compilador conserva información de tipo.

El ejemplo anterior es una forma básica de flexibilidad, pero no es programación genérica real. Usar `void*` en C o `Object` en Java da flexibilidad de almacenamiento, pero no ofrece seguridad en tiempo de compilación ni preserva el tipo específico de los elementos.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas.

### Respuesta

El principal problema es que se pierde el chequeo de tipos en tiempo de compilación. Con `void*` en C y `Object` en Java, cualquier dato puede almacenarse, y al recuperar el valor es necesario convertirlo al tipo esperado. Si se comete un error, el fallo aparece en tiempo de ejecución, no en compilación.

Además, en Java se pierde la información específica del tipo y se obliga a usar conversiones explícitas (`cast`), lo que incrementa el riesgo de `ClassCastException`. En C, el uso de `void*` es inseguro porque el compilador no comprueba la compatibilidad de tipos y no protege frente a errores de interpretación de memoria.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**?

### Respuesta

Los parámetros de tipo son variables que se utilizan en la definición de clases, interfaces o métodos para representar el tipo de datos con el que van a trabajar. En lugar de fijar un tipo concreto, se utiliza una letra como `T`, `E` o `K`, y el usuario decide el tipo al instanciar la clase o invocar el método.

Esto permite escribir colecciones y algoritmos independientes del tipo de dato, pero que conservan la seguridad del compilador. Los parámetros de tipo hacen posible que el código sea genérico y aún así ofrezca comprobación de tipo en tiempo de compilación.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta

En Java, los `generics` permiten declarar una lista que solo admite `String`, manteniendo la seguridad de tipo en tiempo de compilación. En C++, las plantillas (`templates`) ofrecen un comportamiento similar, con la ventaja de generar código específico para cada tipo.

```java
List<String> lista = new ArrayList<>();
lista.add("uno");
lista.add("dos");
for (String valor : lista) {
    System.out.println(valor.toUpperCase());
}
```

```cpp
#include <vector>
#include <string>
#include <iostream>

std::vector<std::string> lista;
lista.push_back("uno");
lista.push_back("dos");
for (const std::string& valor : lista) {
    std::cout << valor << std::endl;
}
```

En ambos casos, el compilador sabe que cada elemento es `String`/`std::string` y no es necesario realizar conversiones manuales al recorrer la colección.

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta

C++ y Java tratan los parámetros de tipo de forma diferente. En C++, el compilador genera código específico para cada instancia de plantilla, un proceso llamado instanciación de plantillas. Por ejemplo, `std::vector<int>` y `std::vector<std::string>` se compilan como implementaciones separadas.

Java utiliza type erasure. El compilador comprueba los tipos genéricos en tiempo de compilación, pero luego elimina la información de tipo en el bytecode, reemplazándola por los tipos de su límite (normalmente `Object`) y añadiendo conversiones cuando sean necesarias. Esto hace que en Java no exista código distinto para cada tipo genérico, mientras que en C++ sí.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`.

### Respuesta

Una clase genérica `Par` puede recibir dos parámetros de tipo diferentes, por ejemplo `A` y `B`. De este modo se puede almacenar un valor de cada tipo y acceder a ellos con seguridad de tipo en tiempo de compilación.

```java
class Par<A, B> {
    private final A primero;
    private final B segundo;

    public Par(A primero, B segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public A getPrimero() {
        return primero;
    }

    public B getSegundo() {
        return segundo;
    }
}

public class Estadistica {
    public static Par<Double, Double> calcularMediaYDesviacion(double[] datos) {
        double suma = 0;
        for (double valor : datos) {
            suma += valor;
        }
        double media = suma / datos.length;
        double varianza = 0;
        for (double valor : datos) {
            varianza += Math.pow(valor - media, 2);
         }
        double desviacion = Math.sqrt(varianza / datos.length);
        return new Par<>(media, desviacion);
    }
}
```

Este `Par` permite devolver dos valores con tipos distintos a la vez, manteniendo la información de tipo para quien lo consume.

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo.

### Respuesta

Un método genérico permite declarar parámetros de tipo específicos para ese método sin necesidad de que la clase sea genérica. Con ello se evita el uso de `Object` y el downcasting, y al mismo tiempo se garantiza que los dos argumentos son del mismo tipo.

```java
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() < 0.5 ? a : b;
}
```

Si se define con `Object`, el código podría ser:

```java
public static Object seleccionaUno(Object a, Object b) {
    return Math.random() < 0.5 ? a : b;
}
```

Con `Object` se pierde información de tipo y el resultado debe convertirse al tipo esperado. Con parámetros de tipo, se evita el downcasting y el compilador exige que ambos argumentos pertenezcan al mismo tipo concreto.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta

Sí, en Java se pueden restringir los parámetros de tipo usando `extends`. Para que un parámetro de tipo represente un número, se puede usar `T extends Number`. Esto permite llamar a métodos disponibles en `Number`, como `doubleValue()`.

Solución simple con `Number`:

```java
class PuntoNumero {
    private final Number x;
    private final Number y;

    public PuntoNumero(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    public double calcularDistanciaA(PuntoNumero otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Solución con generics:

```java
class Punto<T extends Number> {
    private final T x;
    private final T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = x.doubleValue() - otro.x.doubleValue();
        double dy = y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Debido al type erasure de Java, el tipo final tras la compilación es `Punto` con `Number` como límite, y la información de `T` no se mantiene en el bytecode. El chequeo sigue existiendo en tiempo de compilación, pero en ejecución el JVM ve la clase sin el parámetro genérico.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta

La solución con `Number` permite que un punto tenga una coordenada `Integer` y otra `Double`, porque ambas son subtipos de `Number`. Esa flexibilidad es útil, pero no conserva el tipo concreto de cada coordenada al nivel de la clase.

En cambio, la solución genérica `Punto<T extends Number>` obliga a que ambas coordenadas sean del mismo subtipo `T`. Esto refuerza el chequeo de tipos porque si se crea un `Punto<Integer>`, ambas coordenadas serán `Integer` y no se podrán mezclar con `Double`.

En la solución sin generics, `getX()` devuelve `Number`. En la solución con generics, `getX()` devuelve `T`, por lo que si la clase se instancia como `Punto<Integer>` devuelve `Integer`, y si se instancia como `Punto<Double>` devuelve `Double`.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.

### Respuesta

Para evitar `instanceof` y downcasting, se puede definir la interfaz con un parámetro de tipo auto-referencial. Así cada implementación obliga a usar el mismo tipo en el argumento de distancia.

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T otro);
}

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D otro) {
        return Math.sqrt(Math.pow(x - otro.x, 2) + Math.pow(y - otro.y, 2));
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D otro) {
        return Math.sqrt(
            Math.pow(x - otro.x, 2) +
            Math.pow(y - otro.y, 2) +
            Math.pow(z - otro.z, 2)
        );
    }
}
```

Este diseño garantiza en compilación que `Punto2D` solo calcula distancias a otro `Punto2D`, y `Punto3D` solo a otro `Punto3D`. No se necesita `instanceof`, y el compilador evita mezclas de tipos incompatibles.

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta

No, `List<String>` no es subtipo de `List<Object>`. Los tipos genéricos en Java son invariantes: aunque `String` sea subtipo de `Object`, `List<String>` y `List<Object>` son tipos distintos y no se pueden asignar entre sí. Esto evita errores en tiempo de ejecución al intentar insertar elementos de tipos incompatibles.

Sí, `String[]` es subtipo de `Object[]` porque los arrays en Java son covariantes. Esto permite asignar `String[]` a `Object[]`, pero puede provocar una `ArrayStoreException` si se intenta almacenar un objeto que no sea `String` en ese array a través de la referencia de tipo `Object[]`.

Un tipo genérico es:
- covariante cuando acepta subtipos en el mismo sentido que su parámetro (`List<? extends T>`);
- contravariante cuando acepta supertipos en el sentido inverso (`List<? super T>`);
- invariante cuando no acepta ni subtipo ni supertipo (`List<T>`).

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un wildcard (`?`) representa un tipo desconocido en Java generics. Permite declarar colecciones con límites sin fijar un tipo concreto, recuperando flexibilidad sin renunciar totalmente al sistema de tipos.

`List<? extends T>` es un wildcard con límite superior; se usa cuando sólo se necesita leer elementos de la colección como `T` o sus subtipos. `List<? super T>` es un wildcard con límite inferior; se usa cuando se desea insertar elementos de tipo `T` en la colección, y se garantiza que la lista puede contener `T` o sus supertipos.

Ejemplo de suma con `? extends Number`:

```java
public static double sumar(List<? extends Number> lista) {
    double suma = 0;
    for (Number n : lista) {
        suma += n.doubleValue();
    }
    return suma;
}
```

Ejemplo de inserción con `? super Integer`:

```java
public static void agregarEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}
```

Con `? extends` se permite leer valores de forma segura pero no añadir nuevos elementos (excepto `null`). Con `? super` se permite añadir elementos de tipo `T`, pero la lectura solo se puede hacer con seguridad como `Object`.
