<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta

La herencia en orientación a objetos permite que una clase derive de otra, estableciendo una relación "es-un" entre ellas. Esto significa que un objeto de la subclase es también un objeto de la superclase, lo que facilita la reutilización de código y la organización jerárquica. Las dos implicaciones principales son la compatibilidad de tipos, donde los objetos de subclases pueden ser tratados como objetos de la superclase, y la herencia de estado y comportamiento, donde las subclases adquieren automáticamente los atributos y métodos de la superclase.

En el ejemplo, se define una clase Soldado con un atributo privado nombre y un método saludar que imprime el nombre. Las subclases Artillero y Zapador heredan estos elementos, añadiendo sus propios atributos específicos: numCohetes para Artillero y numMinas para Zapador, con getters correspondientes. La compatibilidad de tipos se demuestra creando un array de Soldado que contiene instancias de Artillero y Zapador, permitiendo iterar sobre ellos y llamar al método saludar sin necesidad de conocer el tipo específico.

```java
public class Soldado {
    private String nombre;
    
    public Soldado(String nombre) { 
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Artillero extends Soldado {
    private int numCohetes;
    
    public Artillero(String nombre, int numCohetes) {
        super(nombre);
        this.numCohetes = numCohetes;
    }
    
    public int getNumCohetes() {
        return numCohetes;
    }
}

public class Zapador extends Soldado {
    private int numMinas;
    
    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }
    
    public int getNumMinas() {
        return numMinas;
    }
}

// Uso:
Soldado[] soldados = {new Artillero("Juan", 5), new Zapador("Pedro", 3), new Soldado("Ana")};
for (Soldado s : soldados) {
    s.saludar();
}
```


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

Al crear una instancia de una subclase, se ejecutan dos constructores: primero el de la superclase y luego el de la subclase. El orden es importante porque la inicialización de la superclase debe completarse antes de que la subclase pueda inicializar sus propios atributos. Dentro de un constructor de subclase, `super` se utiliza para invocar explícitamente el constructor de la superclase, permitiendo pasar parámetros necesarios para su inicialización.

Si la clase base no tiene un constructor sin parámetros visible, es obligatorio llamar a `super` con los parámetros requeridos en el constructor de la subclase. Esto asegura que la superclase se inicialice correctamente, ya que Java no proporciona automáticamente un constructor por defecto si se define uno con parámetros. Omitir esta llamada resultaría en un error de compilación.

```java
public class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
        System.out.println("Constructor de Soldado ejecutado para: " + nombre);
    }

    public void saludar() {
        System.out.println("¡Saludos! Soy " + nombre);
    }
}

public class Artillero extends Soldado {
    private int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre);  // Llama al constructor de la superclase
        this.numCohetes = numCohetes;
        System.out.println("Constructor de Artillero ejecutado con " + numCohetes + " cohetes");
    }

    public int getNumCohetes() {
        return numCohetes;
    }
}

public class Zapador extends Soldado {
    private int numExplosivos;

    public Zapador(String nombre, int numExplosivos) {
        super(nombre);  // Llama al constructor de la superclase
        this.numExplosivos = numExplosivos;
        System.out.println("Constructor de Zapador ejecutado con " + numExplosivos + " explosivos");
    }

    public int getNumExplosivos() {
        return numExplosivos;
    }
}

// Ejemplo de uso:
public class Main {
    public static void main(String[] args) {
        Artillero artillero = new Artillero("Juan", 5);
        // Salida:
        // Constructor de Soldado ejecutado para: Juan
        // Constructor de Artillero ejecutado con 5 cohetes

        Zapador zapador = new Zapador("Pedro", 3);
        // Salida:
        // Constructor de Soldado ejecutado para: Pedro
        // Constructor de Zapador ejecutado con 3 explosivos
    }
}
```

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

Los atributos privados de la superclase forman parte de la instancia de la subclase en memoria, ya que la herencia implica que la subclase incluya todos los atributos de la superclase. Sin embargo, esto no implica que puedan ser accedidos directamente desde el código de la subclase, debido a las reglas de encapsulación. Los atributos privados permanecen inaccesibles fuera de la clase que los define, incluso para subclases.

En el ejemplo de Soldado, el atributo nombre es privado, por lo que en Artillero o Zapador no se puede acceder directamente a nombre. Para utilizarlo, se debe recurrir a métodos públicos de la superclase, como saludar, que internamente usa el atributo privado. Esto mantiene la integridad de la encapsulación mientras permite la herencia de estado.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta

La compatibilidad de tipos en herencia implica que el código escrito para trabajar con la superclase puede operar con cualquier subclase sin modificaciones, lo que mejora la extensibilidad. Esto permite añadir nuevos tipos de subclases sin alterar el código existente que utiliza referencias de la superclase. En términos prácticos, significa que se puede extender la funcionalidad agregando nuevas clases derivadas sin romper el código que ya funciona.

Para ilustrarlo, se añade un nuevo tipo Soldado llamado Infante, que hereda de Soldado. El código que recorre un array de Soldado y llama a saludar permanece idéntico, ya que todos los tipos son compatibles con Soldado. Esto demuestra cómo la herencia facilita la adición de nuevas funcionalidades sin requerir cambios en el código cliente.

```java
public class Infante extends Soldado {
    public Infante(String nombre) {
        super(nombre);
    }
}

// El código existente no cambia:
Soldado[] soldados = {new Artillero("Juan", 5), new Zapador("Pedro", 3), new Infante("Ana")};
for (Soldado s : soldados) {
    s.saludar();
}
```


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

En Java, una referencia del supertipo puede apuntar a objetos de un subtipo, lo que se conoce como upcasting y ocurre implícitamente. Sin embargo, con una referencia del supertipo solo se pueden invocar métodos definidos en el supertipo, no los específicos del subtipo, a menos que se realice un downcasting explícito. El upcasting convierte una referencia de subtipo a supertipo, mientras que el downcasting hace lo contrario, requiriendo una verificación previa para evitar errores.

El operador instanceof se utiliza para comprobar si un objeto es una instancia de una clase específica, permitiendo realizar downcasting de forma segura. En el ejemplo, al recorrer un array de Soldado, se usa instanceof para identificar Artillero y luego hacer downcasting para acceder a getNumCohetes.

```java
Soldado[] soldados = {new Artillero("Juan", 5), new Zapador("Pedro", 3)};
for (Soldado s : soldados) {
    if (s instanceof Artillero a) { // downcasting
        System.out.println("Cohetes: " + a.getNumCohetes());
    }
}
```


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta

El acceso protegido en Java, indicado por la palabra clave protected, permite que atributos y métodos sean accesibles dentro de la misma clase, subclases y clases del mismo paquete. Esto proporciona un nivel intermedio entre privado y público, facilitando la herencia al permitir que las subclases accedan a ciertos elementos internos de la superclase sin exponerlos al exterior.

En el ejemplo, cambiando nombre a protected en Soldado, permite que Zapador acceda directamente a nombre en su método ponerMinas, sin necesidad de getters. Esto mantiene la encapsulación frente a clases externas mientras permite flexibilidad en la jerarquía de herencia.

```java
public class Soldado {
    protected String nombre; // Cambiado a protected
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Zapador extends Soldado {
    private int numMinas;
    
    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }
    
    public void ponerMinas() {
        System.out.println(nombre + " pone " + numMinas + " minas");
    }
}
```


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta

En muchos lenguajes orientados a objetos, existe una clase base común para todos los objetos, que proporciona métodos fundamentales como toString o equals. Sin embargo, esto no ocurre en todos los lenguajes; algunos permiten objetos sin una superclase común. En Java, la clase Object es la raíz de la jerarquía, de la cual todas las clases heredan implícitamente, asegurando que todos los objetos compartan un conjunto básico de comportamientos.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta

La herencia múltiple permite que una clase herede de más de una superclase directamente, combinando atributos y métodos de múltiples padres. Esto puede llevar a conflictos si ambas superclases tienen métodos con el mismo nombre.
En Java, no existe herencia múltiple de clases para evitar estos problemas de ambigüedad, pero se puede simular mediante interfaces, que permiten implementar múltiples contratos sin herencia de implementación.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta

Las excepciones personalizadas en Java se crean extendiendo clases de excepción existentes. Para una excepción no controlada, se extiende RuntimeException. La clase UsuarioNoEncontradoException incluye un campo Usuario para identificar el problema y constructores sobrecargados para incluir la causa.

```java
public class Usuario {
    private String nombre;
    
    public Usuario(String nombre) {
        this.nombre = nombre;
    }
    
    public String getNombre() {
        return nombre;
    }
}

public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;
    
    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }
    
    public UsuarioNoEncontradoException(Usuario usuario, Throwable cause) {
        super("Usuario no encontrado: " + usuario.getNombre(), cause);
        this.usuario = usuario;
    }
    
    public Usuario getUsuario() {
        return usuario;
    }
}
```


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta

La herencia debe usarse cuando existe una relación "es-un" genuina, no solo para reutilizar código. Usar herencia únicamente para compartir código puede crear jerarquías artificiales que complican el mantenimiento. La composición, en cambio, permite reutilizar código de forma más flexible mediante la inclusión de objetos, sin imponer una relación de herencia rígida.

Esto evita problemas como la fragilidad de la jerarquía, donde cambios en la superclase afectan a todas las subclases. La composición favorece el principio de responsabilidad única y facilita las pruebas unitarias al desacoplar las dependencias.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta

Se favorece la composición porque ofrece mayor flexibilidad y menor acoplamiento que la herencia. Con composición, se pueden cambiar comportamientos en tiempo de ejecución mediante la inyección de diferentes objetos, mientras que la herencia fija el comportamiento en tiempo de compilación. Además, evita problemas como el diamante de la herencia múltiple y facilita la extensión sin modificar clases existentes.

La composición promueve el principio de "programar contra interfaces", lo que resulta en código más modular y fácil de mantener. Permite combinar funcionalidades de múltiples fuentes sin las restricciones de una única jerarquía de herencia.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta

La herencia rompe la encapsulación porque las subclases pueden acceder a elementos protegidos de la superclase, exponiendo detalles internos que deberían permanecer ocultos. Esto crea una dependencia fuerte entre la superclase y sus subclases, donde cambios en la implementación interna de la superclase pueden afectar a las subclases de formas inesperadas.

En contraste, la composición mantiene la encapsulación al interactuar con objetos a través de interfaces públicas, sin acceder a detalles internos. Esto reduce el riesgo de efectos secundarios y facilita la evolución independiente de las clases.

EDIT: Solo usar herencia cuando necesitas la compatibilidad de tipos.
Usar herencia implica un fuerte acoplamiento desde la clase derivada hacia la clase base.
La clase derivada depende mucho de la base
Cambions internos en la clase base podrian llegar a afectar a la derivada.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta

En el enfoque de herencia, se crea una superclase Persona con dni y nombre, de la cual Estudiante y Trabajador heredan. Esto establece una relación "es-un", pero puede ser problemático si no hay una relación semántica fuerte.

En el enfoque de composición, se define DatosPersonales como una clase separada con dni y nombre. Estudiante y Trabajador reciben una instancia de DatosPersonales en su constructor, permitiendo compartir datos sin herencia. Este método es más flexible y evita acoplamientos innecesarios.

```java
// Herencia
public class Persona {
    protected String dni;
    protected String nombre;
    
    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

public class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

public class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}

// Composición
public class DatosPersonales {
    private String dni;
    private String nombre;
    
    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    
    public String getDni() { return dni; }
    public String getNombre() { return nombre; }
}

public class Estudiante {
    private DatosPersonales datos;
    
    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}

public class Trabajador {
    private DatosPersonales datos;
    
    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}
```
