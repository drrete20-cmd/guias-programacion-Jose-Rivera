<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta

En C, la composición se puede expresar mediante `struct` anidada. Un `Punto` contiene coordenadas `x` e `y` y una `Linea` contiene dos `Punto`. Esta forma de componer es evidente: la línea tiene dos puntos y su vida es parte de la línea.

Un ejemplo de código sería:

```c
#include <math.h>
#include <stdio.h>

typedef struct {
    double x;
    double y;
} Punto;

typedef struct {
    Punto p1;
    Punto p2;
} Linea;

static double distancia(Punto a, Punto b) {
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx*dx + dy*dy);
}

static double longitud(Linea l) {
    return distancia(l.p1, l.p2);
}

int main(void) {
    Linea l = { {0.0, 0.0}, {3.0, 4.0} };
    printf("Longitud: %f\n", longitud(l));
    return 0;
}
```

En este diseño, `Linea` posee explícitamente los dos `Punto`. No hay encapsulación de acceso propio de objetos, pero el patrón es básico y funcional.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

En Java, encapsulamos datos y comportamientos en clases. `Punto` es inmutable porque sus campos son `private final` y no hay setters. Esto mejora la seguridad respecto a C, donde cualquiera accede a miembros `struct` directamente.

```java
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    public double distancia(Punto otro) {
        double dx = x - otro.x;
        double dy = y - otro.y;
        return Math.hypot(dx, dy);
    }
}

public final class Linea {
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        if (inicio == null || fin == null) {
            throw new IllegalArgumentException("Puntos no pueden ser nulos");
        }
        this.inicio = inicio;
        this.fin = fin;
    }

    public Punto getInicio() { return inicio; }
    public Punto getFin() { return fin; }

    public double longitud() {
        return inicio.distancia(fin);
    }
}
```

La clase `Linea` también es inmutable: se definen los dos puntos en el constructor y no cambia su referencia después. Esto consigue la invariancia de que la línea no puede modificar su extremo después de crearse.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

Multiplicidad describe cuántas instancias de un tipo se relacionan con cuántas instancias de otro tipo. En UML se suele escribir como `1`, `0..*`, `1..*`, `2` etc.

En el ejemplo `Linea` y `Punto`, de `Linea` a `Punto` la multiplicidad es fija `2` (o `2..2`), porque una línea se define con exactamente dos puntos. De `Punto` a `Linea`, si no se mantiene referencia inversa, es `0..*`: un punto puede no pertenecer a ninguna línea o pertenecer a varias dependiendo del diseño. En el diseño simple sin referencia inversa, un `Punto` no conoce las líneas en las que participa, así que se trata como `0..*` en sentido lógico.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

Composición fuerte (también llamada agregación fuerte o composición propiedad) implica que el ciclo de vida del componente depende del contenedor: si el objeto contenedor se destruye, sus componentes ya no tienen sentido y desaparecen. En Java esto se traduce en que el contenedor crea y mantiene la referencia de los componentes, y no hay otra entidad responsable de ellos.

Composición débil (a menudo llamada agregación o asociación) implica relaciones más sueltas: el componente puede sobrevivir sin el contenedor, y puede ser compartido por varios contenedores. No se asume control exclusivo del ciclo de vida.

En UML, la composición dura y fuerte se representa con un rombo negro, y la agregación/ asociación con un rombo vacío o línea normal. En el lenguaje cotidiano, se suele llamar "composición" a la fuerte, y "asociación/ agregación" a la débil.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

Se trata de dependencia. Una clase que usa otra a través de parámetros, retornos o variables locales es dependiente, pero no propietaria. En dependencia no se establece una relación de propiedad ni de ciclo de vida.

Dependencia es una relación transitoria: se necesita al otro para realizar una operación, pero no hay necesariamente una unión de partes permanentes o anidamiento a largo plazo.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

Composición fuerte: `Linea` crea sus propios puntos internamente y no expone formas de cambiarlos. El contenedor asume propiedad y la vida de los puntos se gestiona con la vida de línea.

```java
public final class LineaFuerte {
    private final Punto inicio;
    private final Punto fin;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }

    public double longitud() { return inicio.distancia(fin); }
}
```

Composición débil: `Linea` recibe puntos creados por el cliente y los utiliza, sin propiedad exclusiva. Los puntos pudieron existir antes o después de la línea.

```java
public final class LineaDebil {
    private final Punto inicio;
    private final Punto fin;

    public LineaDebil(Punto inicio, Punto fin) {
        if (inicio == null || fin == null) throw new IllegalArgumentException();
        this.inicio = inicio;
        this.fin = fin;
    }

    public double longitud() { return inicio.distancia(fin); }
}
```

En el primer caso, la vida lógica de `Punto` depende de `LineaFuerte`; en el segundo la vida es independiente.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

Java no tiene destructor explícito; el recolector de basura administra memoria. En composición fuerte, si el contenedor (por ejemplo `Linea`) deja de alcanzarse, y ninguna otra referencia a los componentes existe, todo queda elegible para GC.

Por eso no hay llamada explícita a destrucción. Es el tiempo de vida de las referencias el que determina el ciclo de vida de los objetos y en composición fuerte el contenedor mantiene esas referencias.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

La relación es débil porque los profesores pueden existir fuera de un departamento y pueden cambiar de departamento. El departamento mantiene su propia colección de profesores y un director elegido dentro de esa colección.

```java
public final class Profesor {
    private final String nombre;
    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) throw new IllegalArgumentException("Nombre inválido");
        this.nombre = nombre;
    }
    public String getNombre() { return nombre; }
}

public final class Departamento {
    private static final int MAX = 50;
    private final Profesor[] profesores = new Profesor[MAX];
    private int cantidad;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) throw new IllegalArgumentException("Debe haber director");
        this.director = directorInicial;
        profesores[cantidad++] = directorInicial;
    }

    public int getCantidadProfesores() { return cantidad; }

    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= cantidad) throw new IndexOutOfBoundsException("Posición fuera de rango");
        return profesores[pos];
    }

    public void agregarProfesor(Profesor prof) {
        if (prof == null) throw new IllegalArgumentException("Profesor nulo");
        if (cantidad == MAX) throw new IllegalStateException("Máximo alcanzado");
        profesores[cantidad++] = prof;
    }

    public void eliminarProfesor(int pos) {
        if (pos < 0 || pos >= cantidad) throw new IndexOutOfBoundsException("Posición fuera de rango");
        Profesor eliminado = profesores[pos];
        if (eliminado.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        System.arraycopy(profesores, pos + 1, profesores, pos, cantidad - pos - 1);
        profesores[--cantidad] = null;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) throw new IllegalArgumentException("Director nulo");
        int idx = -1;
        for (int i = 0; i < cantidad; ++i) {
            if (profesores[i].equals(nuevoDirector)) {
                idx = i; break;
            }
        }
        if (idx < 0) throw new IllegalArgumentException("El director debe pertenecer al departamento");
        director = nuevoDirector;
    }

    public Profesor getDirector() { return director; }
}
```

La invariancia se mantiene al impedir eliminar al director y al forzar que el nuevo director ya sea miembro del departamento. Este modelo preserva la composición débil de dependencias compartidas.


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

Con `List` se elimina la gestión manual del tamaño y el desplazamiento de elementos, por lo que el código es más corto y fácil de mantener. No se necesita `MAX` ni `System.arraycopy`, ni comprobación de capacidad.

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public final class DepartamentoLista {
    private final List<Profesor> profesores = new ArrayList<>();
    private Profesor director;

    public DepartamentoLista(Profesor directorInicial) {
        if (directorInicial == null) throw new IllegalArgumentException("Debe haber director");
        this.director = directorInicial;
        profesores.add(directorInicial);
    }

    public int getCantidadProfesores() { return profesores.size(); }

    public Profesor getProfesor(int pos) { return profesores.get(pos); }

    public void agregarProfesor(Profesor prof) {
        if (prof == null) throw new IllegalArgumentException("Profesor nulo");
        profesores.add(prof);
    }

    public void eliminarProfesor(int pos) {
        Profesor eliminado = profesores.get(pos);
        if (eliminado.equals(director)) throw new IllegalStateException("No se puede eliminar al director");
        profesores.remove(pos);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null || !profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El director debe pertenecer al departamento");
        }
        director = nuevoDirector;
    }

    public Profesor getDirector() { return director; }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(new ArrayList<>(profesores));
    }
}
```

El problema de devolver la lista interna directamente es que rompe encapsulación y permite modificaciones externas (invariante violado). Se soluciona devolviendo una copia defensiva o una vista inmodificable.


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

La composición recursiva ocurre cuando una clase contiene una instancia de sí misma (directa o indirecta). En `Persona`, el campo `madre` puede ser otra `Persona`, creando una jerarquía familiar. Inmutabilidad se garantiza con campos finales y sin setters.

```java
public final class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) throw new IllegalArgumentException("Nombre requerido");
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() { return nombre; }
    public Persona getMadre() { return madre; }
}

public class MainFamilia {
    public static void main(String[] args) {
        Persona abuela = new Persona("Ana", null);
        Persona madre = new Persona("Bea", abuela);
        Persona nieto = new Persona("Carlos", madre);

        System.out.println("Nieto: " + nieto.getNombre());
        System.out.println("Madre: " + nieto.getMadre().getNombre());
        System.out.println("Abuela: " + nieto.getMadre().getMadre().getNombre());
    }
}
```

Otros ejemplos clásicos: nodos de árbol (árbol binario, árbol general), listas enlazadas (`Nodo` con referencia a siguiente), y excepciones encadenadas (`Throwable.cause`).


## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Bidireccional significa que cada lado conoce al otro: el departamento conoce sus profesores y cada profesor sabe a qué departamento pertenece. Esta relación requiere mantener coherencia en ambos sentidos para evitar datos inconsistentes.

Para el ejemplo, `Profesor` tendría un campo `Departamento departamento`, inicializado al agregarse. El `Departamento` tendría métodos que actualicen ambas partes, por ejemplo `agregarProfesor(Profesor p)` asigna `p.departamento = this` y `p.setDepartamento(this)` (inmutable con nueva instancia o método package). Al eliminar, se debe borrar la referencia del profesor al departamento. La clave es no permitir estados intermedios donde el profesor no coincida con la lista del departamento.

Un patrón habitual es usar métodos de fábrica o package private para que solo `Departamento` pueda cambiar la referencia del profesor, y así preservar la invariancia bidireccional.
