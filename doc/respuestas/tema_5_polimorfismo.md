<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta

El polimorfismo es la capacidad de un objeto de adoptar múltiples formas, permitiendo que un mismo método tenga diferentes comportamientos según el tipo específico del objeto que lo ejecute. En Java, esto significa que una referencia de tipo base puede apuntar a objetos de tipos derivados, y el método que se ejecuta se determina en tiempo de ejecución según el tipo real del objeto. Esto permite escribir código más genérico y flexible: se pueden recorrer colecciones de objetos de diferentes tipos y llamar a sus métodos sin necesidad de saber exactamente qué tipo específico es cada uno.

La sobreescritura (overriding) de métodos es el mecanismo fundamental que hace posible el polimorfismo. Ocurre cuando una clase derivada redefine un método que hereda de su clase base, proporcionando una implementación específica para su tipo. Este método debe tener la misma firma (nombre, parámetros y tipo de retorno, o compatible) que el método base. Cuando se llama al método a través de una referencia de tipo base, Java ejecuta la versión del método correspondiente al tipo real del objeto, no al tipo de la referencia.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta

La ligadura dinámica (dynamic binding) o enlace tardío (late binding) es el proceso mediante el cual Java determina qué versión de un método ejecutar en el momento en que el programa está corriendo, basándose en el tipo real del objeto, no en el tipo de la referencia. A diferencia de en C++ donde esto debe ser explícitamente indicado con la palabra clave `virtual`, en Java este comportamiento es el comportamiento por defecto para los métodos no estáticos ni finales. Esto significa que en Java el polimorfismo funciona automáticamente sin necesidad de hacer nada especial al programar.

En C++, sin la palabra clave `virtual` se utiliza ligadura estática (static binding), donde el método a ejecutar se determina según el tipo de la referencia en tiempo de compilación. Con `virtual`, C++ cambia a ligadura dinámica. En Java, todos los métodos de instancia utilizan ligadura dinámica automáticamente, lo que hace que sea mucho más sencillo trabajar con polimorfismo. Python también utiliza ligadura dinámica por defecto, siendo incluso más flexible que Java al no requerir herencia: cualquier objeto con un método puede ser utilizado donde se espera ese método (duck typing).


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta

El siguiente código muestra cómo implementar polimorfismo con la clase base `Soldado` y sus subclases `Zapador` y `Artillero`:

```java
class Soldado {
    public void saludar() {
        System.out.println("Soldado a sus órdenes");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador reportándose");
    }
}

class Artillero extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Artillero en posición");
    }
}

public class PolimorfismoEjemplo {
    public static void main(String[] args) {
        Soldado[] escuadron = new Soldado[3];
        escuadron[0] = new Soldado();
        escuadron[1] = new Zapador();
        escuadron[2] = new Artillero();
        
        for (Soldado soldado : escuadron) {
            soldado.saludar();
        }
    }
}
```

En este ejemplo se crea un array de tipo `Soldado`, pero se almacenan objetos de diferentes tipos. Al recorrer el array y llamar a `saludar()`, Java invoca la versión correcta del método según el tipo real de cada objeto, no según el tipo de la referencia, que es `Soldado`. Esto demuestra el polimorfismo en acción: el mismo método invocado en todas las iteraciones produce diferentes resultados.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta

Sí, es posible invocar el método de la clase base utilizando la palabra clave `super`. En el caso de `Zapador`, se puede hacer que salude primero de forma normal y luego añada su información especial:

```java
class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar();  // Invoca el método de Soldado
        System.out.println("ZAPADOR A SUS ÓRDENES");
    }
}
```

La palabra clave `super` permite acceder a miembros y métodos de la clase base desde una clase derivada. Esto es especialmente útil cuando se necesita reutilizar parte de la lógica del método base y extenderla con comportamiento adicional. Sin `super`, se tendría que repetir todo el código del método base en cada subclase, violando el principio DRY (Don't Repeat Yourself).


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta

Al sobreescribir un método en Java, existen restricciones sobre los tipos: el tipo de retorno debe ser el mismo o un subtipo del tipo de retorno del método base (covarianza de tipos), y los tipos de los parámetros deben ser exactamente iguales. Si los parámetros fueran diferentes, estaría ocurriendo sobrecarga, no sobreescritura. La sobreescritura (overriding) ocurre cuando una subclase redefine un método heredado con la misma firma; la sobrecarga (overloading) ocurre cuando hay múltiples métodos con el mismo nombre pero diferentes parámetros en la misma clase.

La anotación `@Override` es una anotación de compilación que le indica al compilador que el siguiente método debe sobreescribir un método de la clase base. Si el compilador detecta que el método no sobreescribe nada (por ejemplo, si hay un error tipográfico en el nombre), genera un error de compilación. Es recomendable usarla siempre porque ayuda a prevenir errores sutiles y hace el código más legible, indicando claramente la intención del programador de sobreescribir un método existente.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta

Sí, cuando se sobrescribe `toString()` u `equals()` ya se está utilizando polimorfismo. Estos métodos están definidos en la clase `Object`, que es la clase base de todas las clases en Java. Al sobreescribirlos en una clase propia, se está participando en el mecanismo de polimorfismo: cuando código genérico (como el método `println` que utiliza `toString()`) llama a estos métodos sobre referencias de tipo `Object`, Java ejecuta la versión específica de la clase, no la de `Object`.

Este polimorfismo es invisible para muchos estudiantes inicialmente, pero está presente desde las primeras lecciones. Por ejemplo, cuando se crea una clase propia y se sobrescribe `toString()`, y luego se imprime un objeto con `System.out.println()`, el sistema está utilizando ligadura dinámica para llamar a la versión correcta de `toString()`. Todas las clases que definen especialidades de `toString()` o `equals()` están aprovechando el polimorfismo sin necesidad de hacerlo explícito o consciente.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta

Una clase abstracta es una clase que no puede ser instanciada directamente y está pensada para servir como clase base para otras clases. Un método abstracto es un método que no tiene implementación en la clase abstracta; las subclases concretas están obligadas a proporcionar una implementación. Una clase abstracta se declara con la palabra clave `abstract`, y sus métodos abstractos también se marcan con `abstract` y terminan con punto y coma en lugar de un cuerpo. Los métodos normales (concretos) en una clase abstracta sí pueden tener implementación.

En el ejemplo del `Soldado`, la clase se declara `abstract` junto con el método `atacar()`:

```java
abstract class Soldado {
    public void saludar() {
        System.out.println("Soldado a sus órdenes");
    }
    
    abstract void atacar();
}

class Zapador extends Soldado {
    @Override
    void atacar() {
        System.out.println("Zapador abre trincheras");
    }
}

class Artillero extends Soldado {
    @Override
    void atacar() {
        System.out.println("Artillero dispara");
    }
}
```

La palabra clave `abstract` se coloca tanto en la declaración de la clase como en la declaración del método que no tiene implementación. Las subclases concretas como `Zapador` y `Artillero` deben proporcionar una implementación para el método `atacar()`, de lo contrario también serían abstractas. No se puede crear una instancia de `Soldado` directamente; debe hacerse a través de sus subclases.


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta

La palabra clave `final` prohibe que un método sea sobreescrito en las subclases o que una clase sea extendida. Cuando se aplica a un método, indica que ese método tiene un comportamiento específico que no debe ser modificado por las subclases. Cuando se aplica a una clase completa, indica que es el final de la jerarquía de herencia y ninguna otra clase puede heredar de ella. Esto está directamente relacionado con el polimorfismo: `final` lo restricta, evitando que se utilice ligadura dinámica para ese método o clase.

Ejemplos de clases `final` en la API estándar de Java incluyen `String`, `Integer`, `Double` y otras clases wrapper. Estas clases son `final` por razones de seguridad y rendimiento: `String` es `final` porque es inmutable y crítica para la seguridad; cambiar su comportamiento a través de herencia podría comprometer la integridad del programa. El uso de `final` es una decisión de diseño que debe hacerse conscientemente, priorizando la seguridad sobre la flexibilidad.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta

Las interfaces en Java son contratos que especifican qué métodos debe tener una clase, pero sin proporcionar implementación para esos métodos (aunque en Java 8+ pueden tener métodos por defecto). A diferencia de las clases abstractas, una interfaz representa un "qué puede hacer" más que un "qué es". Una clase abstracta puede tener tanto métodos abstractos como concretos, y puede tener atributos; una interfaz, en su forma clásica, solo define métodos sin implementación.

Sí, una clase puede implementar más de una interfaz, lo que permite una forma de herencia múltiple de tipos. Por ejemplo, una clase puede implementar tanto `Serializable` como `Cloneable`. Las interfaces pueden heredar de otras interfaces usando la palabra clave `extends`. Este es uno de los puntos fuertes de las interfaces: permite que un objeto cumpla múltiples contratos simultáneamente, algo que la herencia simple de clases no permite. Aunque Java permite herencia simple de clases (una clase solo puede extender una clase), permite herencia múltiple de interfaces.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta

El siguiente código implementa un sistema de puntos abstractos que permite calcular distancias sin conocer si los puntos son 2D o 3D:

```java
abstract class Punto {
    abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    double x, y;
    
    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    @Override
    double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Solo puntos 2D");
        }
        Punto2D p = (Punto2D) otro;  // downcasting
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}

class Punto3D extends Punto {
    double x, y, z;
    
    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }
    
    @Override
    double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Solo puntos 3D");
        }
        Punto3D p = (Punto3D) otro;  // downcasting
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2) + Math.pow(z - p.z, 2));
    }
}

class Linea {
    Punto inicio, fin;
    
    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }
    
    double longitud() {
        return inicio.calcularDistanciaA(fin);
    }
}
```

La clase `Linea` demuestra el verdadero poder del polimorfismo: acepta referencias de tipo `Punto` sin necesidad de saber si son `Punto2D` o `Punto3D`. Cuando se llama a `calcularDistanciaA()`, Java invoca el método correcto según el tipo real de los puntos. El operador `instanceof` se utiliza para verificar que los puntos son del mismo tipo antes de realizar el cálculo; el downcasting (conversión forzada) permite acceder a los atributos específicos de cada tipo de punto.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta

La herencia de interfaces en Java permite que una interfaz extienda otra interfaz usando `extends`. Esto permite crear jerarquías de interfaces donde una interfaz más específica hereda todos los métodos de una interfaz más general y añade nuevos métodos. Existe herencia múltiple de interfaces: una interfaz puede extender varias interfaces simultáneamente.

El siguiente ejemplo muestra una interfaz `Fichero` básica que es extendida por `FicheroEscribible`:

```java
interface Fichero {
    String leerContenido();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}

class ArchivoTexto implements FicheroEscribible {
    private String contenido = "";
    
    @Override
    public String leerContenido() {
        return contenido;
    }
    
    @Override
    public void escribir(String texto) {
        contenido = texto;
    }
    
    @Override
    public void eliminar() {
        contenido = "";
        System.out.println("Archivo eliminado");
    }
}
```

Una clase que implementa `FicheroEscribible` debe proporcionar implementación para los tres métodos: `leerContenido()` (heredado de `Fichero`), `escribir()` y `eliminar()`. Esta estructura permite definir niveles de funcionalidad: se puede tener una clase que solo implemente `Fichero` (solo lectura) y otra que implemente `FicheroEscribible` (lectura y escritura), reutilizando código y manteniendo contratos claros.
