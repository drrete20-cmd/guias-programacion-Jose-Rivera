<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta
Una forma trivial es utilizar un valor de retorno especial que indique fallo, y dejar el resultado real mediante un parámetro por referencia. Por ejemplo:

```c
#include <math.h>
#include <stdbool.h>

bool raiz(double x, double *out){
    if(x<0){
        return false; /* error, negativa */
    }
    *out = sqrt(x);
    return true;
}

int main(void){
    double r;
    if(!raiz(-4.0,&r)){
        printf("entrada negativa\n");
    }else{
        printf("raiz=%.2f\n",r);
    }
}
```

Otra opción es devolver el resultado directo y usar una variable global o `errno` para señalar el error.

```c
#include <errno.h>
#include <math.h>
double raiz(double x){
    if(x<0){
        errno=EDOM;
        return 0.0;
    }
    return sqrt(x);
}
int main(void){
    errno=0;
    double r=raiz(-1.0);
    if(errno==EDOM){
        printf("error: numero negativo\n");
    }else{
        printf("raiz=%.2f\n",r);
    }
}
```

Ambas soluciones obligan al llamador a comprobar el estado tras la llamada, ya que la función en sí no puede interrumpir el flujo.



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta
Una excepción es un mecanismo lingüístico para señalar que ha ocurrido una situación anómala durante la ejecución de un programa. Se trata de un objeto o valor que interrumpe el flujo normal y se propaga hasta encontrar un manejador.

El programador usa excepciones para separar el código que realiza una operación de aquel que trata los errores. Cuando implementa una función, puede lanzar una excepción si detecta una condición de error; cuando la llama, puede capturarla para tomar decisiones o dejar que continúe propagándose. El objetivo es facilitar el control de errores de forma estructurada y evitar múltiples comprobaciones manuales.



## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta
```java
public class Calculadora {
    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo");
        }
        return Math.sqrt(x);
    }
    public static void main(String[] args) {
        try {
            double r = raiz(-9);
            System.out.println("Raíz: " + r);
        } catch (IllegalArgumentException e) {
            System.out.println("Error desde main: " + e.getMessage());
        }
    }
}
```

En este código `raiz` lanza una excepción y el `main` la controla con un bloque `try-catch`.



## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta
Lanzar una excepción significa crear un objeto de excepción y transferir el control al runtime con la palabra `throw` (Java) o un mecanismo similar. Controlar o capturar una excepción es interponer un bloque `catch` (o `try`/`except`) que atrape el objeto y ejecute código de recuperación.

Propagar una excepción es dejar que el error suba por la pila de llamadas sin capturarlo. Cada función de la pila que no tiene un bloque `catch` adecuado se abandona inmediatamente; sus variables locales se destruyen y no se reanudan tras la excepción. En el ejemplo anterior, si `main` no tuviera el `try-catch`, la excepción lanzada en `raiz` viajaría hacia arriba, saliendo del método `main` y terminando el programa.

Así, la ejecución no vuelve a las funciones intermedias: una vez que la excepción pasa por ellas se pierden y nunca continúan. Solo la función que finalmente la capture podrá reanudar la ejecución tras el bloque `catch`.



## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta
La propagación natural permite que un error sea tratado en un punto central sin que cada función intermédia necesite comprobar manualmente códigos de retorno. En C el llamador inmediato debe preguntar cada vez; si olvida hacerlo el error se ignora. Con excepciones, si ningún manejador local se ocupa del problema, el runtime se encarga de ascender por la pila hasta encontrar uno adecuado.

Esto reduce el acoplamiento y el código repetitivo, ya que sólo los niveles que saben cómo recuperarse se hacen cargo. Además hace posible implementar políticas transversales (registro, rollback) en un único lugar, simplemente dejando que la excepción suba.



## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta
En la mayoría de lenguajes orientados a objetos, las excepciones son instancias de clases que heredan de una base común. Al ser objetos, pueden encapsular información relevante (mensaje, estado, causa) y los detalles internos quedan ocultos.

Esto respeta el principio de encapsulación: el lanzador no necesita conocer cómo está construido el objeto de excepción, sólo su interfaz. Además permite crear clases de excepción personalizadas que transporten datos adicionales o representen situaciones específicas de la aplicación, heredando de `Exception` o similar.



## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta
Un objeto excepción suele portar al menos un mensaje descriptivo y un trazado de pila (stack trace) que indica dónde se produjo. Esa información es esencial en el manejador porque permite diagnosticar el origen del fallo sin necesidad de que cada función retorne códigos elaborados.

En el ejemplo en C habría que agregar variables o punteros extra para llevar esa información, pero en Java el objeto `IllegalArgumentException` contiene la descripción y la pila, lo que simplifica la gestión y mantiene encapsulado el detalle del error.



## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta
Sí: un bloque `try` puede ir seguido de varios `catch` que capturen distintos tipos de excepción. Cuando se lanza una excepción, el primero de los `catch` cuya cabecera sea compatible con el tipo de la excepción se ejecuta.

Aunque haya varios `catch` escritos, solo uno se ejecuta: el que corresponde al tipo más específico que encaje. Los restantes se omiten totalmente.



## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta
Para asegurar la ejecución de un código de limpieza siempre que se salga del `try` (bien por normalidad o por excepción) se emplea la cláusula `finally`. Ejemplo:

```java
try (/* no usado aquí */) {
    FileInputStream f = new FileInputStream("datos.txt");
    // operación que puede lanzar IOException
} catch (IOException e) {
    System.out.println("Fallo al abrir");
} finally {
    // este bloque se ejecuta tanto si hubo excepción como si no
    System.out.println("cerrar recursos");
}

try {
    FileInputStream f = new FileInputStream("otro.txt");
    // ...
} finally {
    System.out.println("se cierra siempre");
}
```

En el primer caso el `finally` acompaña a un `catch`; en el segundo sólo hay `try` y `finally`. En ambos puntos el contenido de `finally` se ejecutará antes de que la excepción siga propagándose o antes de que el flujo normal continúe.



## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta
Sí, en Java un `finally` puede aparecer acompañado únicamente de un `try`, sin ningún `catch`. El bloque `finally` se ejecuta siempre, tanto si se lanza una excepción dentro del `try` como si el código termina normalmente.

Incluso si el `try` contiene un `return`, el código del `finally` se ejecutará antes de que el método retorne. El valor devuelto puede incluso ser modificado en el `finally`, pero la clausura se ejecuta sin falta.



## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta
Las excepciones controladas (checked) son aquellas que el compilador obliga a declarar o manejar; heredan de `Exception` excepto `RuntimeException`. Las no controladas (unchecked) derivan de `RuntimeException` y representan errores de programación que no se pretende forzar a atrapar.

`RuntimeException` funciona como base de estas últimas; su uso indica condiciones como índices fuera de rango o nulos. Ejemplos controlados: `IOException`, `SQLException`, `ClassNotFoundException`. Ejemplos no controlados: `NullPointerException`, `IllegalArgumentException`, `IndexOutOfBoundsException`.

Preferencia por controladas:
- operaciones de E/S en disco
- acceso a bases de datos
- reflexión dinámica
- apertura de sockets

Preferencia por no controladas:
- precondiciones violadas (argumentos inválidos)
- errores de lógica interna
- conversión de tipos incorrecta
- referencias nulas imprevistas



## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta
La palabra clave `throws` en la firma de un método indica que éste puede provocar cierto tipo de excepciones que no maneja y que deben ser gestionadas por el llamador. Es una alternativa a capturarlas dentro del propio método; en lugar de usar `try-catch` se delega la responsabilidad hacia arriba.

De este modo se preserva la abstracción del método: quien lo invoque decidirá si atrapar la excepción o seguir propagándola. El compilador comprueba que los métodos que llaman a uno con `throws` traten la excepción o también la declaren.



## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta
```java
void leerArchivo(String nombre) throws IOException {
    FileInputStream fis = null;
    try {
        fis = new FileInputStream(nombre);
        // leer datos...
    } finally {
        if (fis != null) {
            fis.close();
        }
    }
}

public static void main(String[] args) {
    try {
        leerArchivo("datos.txt");
    } catch (IOException e) {
        System.err.println("no existe el fichero");
    }
}
```

La firma `throws IOException` indica que `leerArchivo` deja pasar la excepción si el fichero no existe. Aun así, se usa `finally` para cerrar el recurso.



## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta
Es legal listar en `throws` excepciones no controladas, porque Java permite cualquier clase `Throwable` en la cláusula. Sin embargo no hay obligación de que el llamador las capture; el compilador no lo exige.

No tiene mucho sentido declarar `throws RuntimeException` salvo como documentación, pues su presencia no cambia nada. El método llamador puede optar por `try-catch` si desea interceptarlas, pero generalmente se deja que se propaguen hasta un manejador global o provoquen la terminación del programa.



## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta
Se recomienda usar excepciones controladas cuando la condición de error depende de factores externos al programa (disco, red, entrada del usuario) y el código que invoca puede razonablemente recuperarse. Las no controladas son apropiadas para errores de programación que deberían corregirse en desarrollo, como pasar null o índices fuera de rango.

Java y otros lenguajes como C# distinguen ambas; Python, JavaScript o C++ sólo tienen excepciones no controladas. En esos lenguajes la única opción es lanzar objetos y confiar en el llamador o en bloques globales. Es habitual en lenguajes dinámicos que no haya chequeo en tiempo de compilación: todas las excepciones son del mismo tipo.



## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta
Dentro de un `catch` puede lanzarse otra excepción, por ejemplo para transformar un error de bajo nivel en uno del dominio de la aplicación:

```java
try {
    // ...
} catch (IOException io) {
    throw new MiExcepcionDeApp("imposible leer", io);
}
```

También es posible relanzar la misma excepción capturada usando `throw e;` después de realizar alguna acción de registro. Esto se usa cuando sólo interesa anotar el error o limpiar recursos, pero no manejarlo:

```java
try {
    procesar();
} catch (Exception e) {
    logger.error("fallo", e);
    throw e; // relanzar
}
```

El relanzamiento mantiene la información original y permite que un nivel superior siga gestionándola.



## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta
Cuando una excepción es la causa de otra, el primer objeto se guarda dentro del segundo mediante el constructor o `initCause`. Esto se usa para envolver un error de bajo nivel dentro de una excepción más genérica.

```java
try {
    Integer.parseInt(texto);
} catch (NumberFormatException nfe) {
    throw new MiError("texto no numérico", nfe);
}
```

Al imprimir la traza de la excepción resultante (`MiError`), el stack trace muestra también la causa (`NumberFormatException`) en una sección etiquetada "Caused by", por lo que se ve en pantalla.


