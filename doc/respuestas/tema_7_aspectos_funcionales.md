<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un puntero a una función en C es una variable que almacena la dirección de memoria de una función. Eso permite llamar a la función a través del puntero, de forma similar a como se usa un puntero a datos para acceder a un valor.

En el siguiente ejemplo se define una función que recibe una cadena y la convierte a mayúsculas. Luego se declara un puntero local `aMayusculas` que apunta a esa función y se invoca usando el puntero.

```c
#include <stdio.h>
#include <ctype.h>

char *convertirAMayusculas(char *texto) {
    for (char *p = texto; *p != '\0'; p++) {
        *p = toupper((unsigned char)*p);
    }
    return texto;
}

int main(void) {
    char mensaje[] = "hola mundo";
    char *(*aMayusculas)(char *) = convertirAMayusculas;
    char *resultado = aMayusculas(mensaje);
    printf("%s\n", resultado);
    return 0;
}
```

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una función lambda es una función anónima que puede definirse en línea y asignarse a una variable. En muchos lenguajes se utiliza para escribir operaciones pequeñas y pasarlas como valores sin necesidad de declarar una función con nombre separado.

En Javascript la función lambda se escribe con la sintaxis de flecha y puede guardarse en `aMayusculas`. En Java, una lambda puede asignarse a un tipo funcional como `Function<String, String>`.

```javascript
const aMayusculas = s => s.toUpperCase();
console.log(aMayusculas('hola mundo'));
```

```java
import java.util.function.Function;

Function<String, String> aMayusculas = s -> s.toUpperCase();
System.out.println(aMayusculas.apply("hola mundo"));
```

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El paradigma funcional es un estilo de programación que trata las funciones como entidades principales y favorece funciones puras, inmutabilidad y composición. En este enfoque, el programa se construye aplicando y combinando funciones, en vez de enfatizar el estado mutable y los objetos.

Lenguajes como Java 8 se llaman multi-paradigma porque admiten tanto programación orientada a objetos como elementos funcionales. Java sigue usando clases y objetos, pero también permite pasar funciones como parámetros, devolverlas y operar con expresiones lambda.

Decir que las funciones son "ciudadanos de primera clase" significa que pueden asignarse a variables, pasarse como argumentos, devolverse desde otras funciones y almacenarse en estructuras de datos. Ese comportamiento es lo que permite usar funciones de forma similar a valores como enteros o cadenas.

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

La sintaxis básica de una lambda en Java es `(parametros) -> expresion` o `(parametros) -> { cuerpo }`. Si la lambda tiene un solo parámetro y su tipo puede inferirse, los paréntesis son opcionales.

Cuando el cuerpo es una sola expresión, no hace falta escribir `return` ni llaves. Si el cuerpo tiene varias sentencias, se usan llaves y es necesario escribir `return` cuando se devuelve un valor.

Por ejemplo:

```java
Function<String, String> aMayusculas = s -> s.toUpperCase();
Function<String, String> repetir = s -> {
    String duplicado = s + s;
    return duplicado;
};
```

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Recibir una función como parámetro permite abstraer el comportamiento de transformación fuera del método. El método `transformar` toma un `String` y una función transformadora, y luego aplica esa función desde su propio cuerpo.

En Javascript el método recibe un callback y lo invoca. En Java se usa `Function<String, String>` para definir el tipo de la función que transforma la cadena.

```javascript
function transformar(texto, transformador) {
    return transformador(texto);
}

const aMayusculas = s => s.toUpperCase();
console.log(transformar('hola', aMayusculas));
```

```java
import java.util.function.Function;

static String transformar(String texto, Function<String, String> transformador) {
    return transformador.apply(texto);
}

Function<String, String> aMayusculas = s -> s.toUpperCase();
System.out.println(transformar("hola", aMayusculas));
```

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Pasar la lambda directamente en la llamada evita declarar una variable adicional y hace el código más compacto. Esta técnica es útil cuando la función se usa solo en ese punto y su comportamiento es simple.

En Javascript se puede entregar la función de inversión como argumento inmediato. En Java se usa la lambda `s -> new StringBuilder(s).reverse().toString()` dentro de la llamada.

```javascript
console.log(transformar('hola', s => s.split('').reverse().join('')));
```

```java
System.out.println(transformar("hola", s -> new StringBuilder(s).reverse().toString()));
```

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un cierre o closure es una función que recuerda el entorno en el que fue creada y puede acceder a variables locales de ese entorno. En Java, la lambda puede usar variables locales siempre que sean efectivamente finales, lo que significa que no cambian después de su asignación.

El siguiente ejemplo muestra una lambda que añade un sufijo definido fuera de la lambda. Esa variable externa queda capturada y se mantiene disponible cada vez que se invoca la función.

```java
String sufijo = "!!!";
Function<String, String> agregarSufijo = s -> s + sufijo;
System.out.println(agregarSufijo.apply("Hola"));
```

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

La diferencia principal es que una lambda puede capturar el contexto donde fue definida, mientras que un puntero a función en C solo contiene la dirección del código. El puntero en C no guarda información adicional sobre variables externas ni sobre el ambiente en el que se creó.

Además, las lambdas en Java y otros lenguajes se tratan como objetos con un tipo funcional, con límites de seguridad del lenguaje. En cambio, los punteros a funciones en C son más básicos y no ofrecen cierre, inferencia de tipos ni la misma comodidad para pasar funciones como valores.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

La función `crearDescuento` devuelve otra función que aplica el porcentaje almacenado en su entorno. Cada función devuelta recuerda el porcentaje con el que fue creada, lo que permite tener descuentos distintos sin reescribir la lógica.

La closure mantiene el valor de `porcentaje` en la lambda incluso después de que el método haya terminado. Es decir, la función devuelta captura la variable local y la usa cada vez que se aplica el descuento.

```java
import java.util.function.Function;

static Function<Double, Double> crearDescuento(double porcentaje) {
    return cantidad -> cantidad * (1 - porcentaje / 100);
}

Function<Double, Double> descuentoDiez = crearDescuento(10);
Function<Double, Double> descuentoVeinte = crearDescuento(20);

System.out.println(descuentoDiez.apply(100.0));
System.out.println(descuentoVeinte.apply(100.0));
```

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

Una interfaz funcional es una interfaz que declara exactamente un método abstracto. Ese método es el único comportamiento que debe implementarse con una lambda, lo que hace posible asociar la expresión lambda a ese tipo.

Los requisitos son: un único método abstracto, y puede tener métodos `default` o `static` adicionales sin afectar la condición. La anotación `@FunctionalInterface` es opcional, pero ayuda a que el compilador valide que la interfaz cumple la condición de un solo método abstracto.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

Una interfaz funcional simple para transformar cadenas define un único método abstracto que toma una cadena y devuelve otra. Esa interfaz puede usarse con lambdas que realicen cualquier transformación sobre textos.

```java
@FunctionalInterface
public interface Transformador {
    String aplicar(String texto);
}
```

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Hacer la interfaz genérica permite reutilizarla con distintos tipos de entrada y salida. De esa forma se puede usar el mismo patrón para transformar `String`, `Double`, `Integer` o cualquier otra clase.

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R aplicar(T valor);
}

Transformador<Double, Integer> redondear = d -> (int) Math.round(d);
System.out.println(redondear.aplicar(3.7));
```

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Java ya incluye varias interfaces funcionales en el paquete `java.util.function`. Las más comunes son `Function<T, R>`, `Consumer<T>`, `Supplier<T>`, `Predicate<T>`, `UnaryOperator<T>` y `BinaryOperator<T>`.

También existen interfaces binarias como `BiFunction<T, U, R>` y `BiConsumer<T, U>`, además de versiones especializadas para tipos primitivos como `IntConsumer`, `IntFunction<R>` o `DoublePredicate`.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

El método `forEach` recibe un `Consumer` y aplica la operación a cada elemento de la lista. Es una forma más declarativa de recorrer colecciones, porque se describe la acción que se quiere realizar en lugar de gestionar el índice o el iterador.

```java
List<Integer> numeros = List.of(-2, 0, 3, 7);
numeros.forEach(n -> {
    if (n > 0) {
        System.out.println("Positivo: " + n);
    }
});
```

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

`Consumer<? super T>` se usa porque un consumidor puede aceptar un tipo `T` o cualquier supertipo de `T`. Eso hace que el método sea más flexible al permitir consumidores que esperan tipos más generales.

PECS significa "Producer Extends, Consumer Super". Cuando se produce un valor se usa `extends`; cuando se consume un valor se usa `super`. En el caso de `transformar`, para aceptar más tipos de funciones transformadoras se podría usar `Function<? super String, ? extends String>`.

Así, el método puede recibir una función que acepte `String` o un supertipo de `String`, y que devuelva `String` o un subtipo de `String`, lo que da mayor compatibilidad sin perder seguridad de tipos.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

En Javascript se puede guardar una referencia al método de un objeto usando `bind` para que `this` conserve el contexto. En Java se puede obtener la referencia con `persona::saludar` y luego invocarla como una lambda o como un `Runnable`.

```javascript
const persona = {
    nombre: 'Ana',
    saludar() {
        return 'Hola ' + this.nombre;
    }
};
const saludar = persona.saludar.bind(persona);
console.log(saludar());
```

```java
public class Persona {
    private final String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public String saludar() {
        return "Hola " + nombre;
    }
}

Persona persona = new Persona("Ana");
Supplier<String> saludar = persona::saludar;
System.out.println(saludar.get());
```

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

En Java hay cuatro tipos de referencias a método: estático, constructor, método de instancia de una instancia concreta y método de instancia sobre cualquier instancia. Cada uno tiene una sintaxis con `::` y se usa según la forma en que se desea enlazar el comportamiento.

```java
Function<String, Integer> parseInt = Integer::parseInt;          // método estático
Supplier<Persona> creador = Persona::new;                       // constructor
Persona persona = new Persona("Luis");
Supplier<String> saludo = persona::saludar;                     // método de instancia de una instancia concreta
Function<Persona, String> obtenerNombre = Persona::saludar;     // método de instancia sobre cualquier instancia
```

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

Para ordenar con `Collections.sort` y una lambda se define el comparador directamente usando `Integer.compare` y `String.compareTo`. Esa versión manual es clara y muestra el control total de la comparación.

La segunda versión usa `Comparator` para expresar el criterio de forma más declarativa y reutilizable.

```java
Collections.sort(personas, (p1, p2) -> {
    int comparacionEdad = Integer.compare(p1.getEdad(), p2.getEdad());
    if (comparacionEdad != 0) {
        return comparacionEdad;
    }
    return p1.getNombre().compareTo(p2.getNombre());
});

Collections.sort(personas,
    Comparator.comparingInt(Persona::getEdad)
              .thenComparing(Persona::getNombre)
);
```

