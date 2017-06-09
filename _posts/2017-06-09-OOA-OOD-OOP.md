---
layout: post
title: OOA, OOD, OOP Concepts (Español)
author: Cesar Gonzalez.
---

Descripcion de que significan los conceptos de OOA, OOD y  OOP para mi.

## OOA, OOD, OOP Concepts 

Soy buen desarrollador, y pongo en mi CV, _experto in OOA, OOD and OOP_ porque es lo &quot;in&quot; (o era).

### ¿Pero que es OOA, OOD y OOP?

Aquí intento dar una pequeña descripción de lo que yo entiendo que son estos conceptos, y como &quot;utilizarlos&quot; o pensar en sus términos cuando diseñamos un sistema.

### Object–Oriented Analysis (OOA)

Detectar los requerimientos, y desarrollar las especificaciones en términos de Object Moidel.

En otras palabras, ver de qué se trata el sistema y hacer uno o más diagramas de como funcionara.

Son modelados sobre objetos de la vida real, para ser representados en sistemas orientados a objetos.

Las tareas que se realizan en análisis son:

- Identificar objetos o actores de la app.
- Crear un Object model diagram.
- Definir las &quot;tripas&quot; del objeto, atributos.
- Definir las acciones del objeto, métodos.
- Describir como los objetos interactúan.

Los pasos que normalmente sigo, y son sugeridos son

1. Preguntarte Que, Porque, Cuando, Como, Quien, Donde, ayuda a obtener información general.
2. Describir los &quot;use cases&quot;, a detalle que describan quien que y que obtiene
3. De los use cases y preguntas anteriores, sacamos los actores.
4. Qué relación tienen entre ellos, cuando y como interactúan, puedes sacar un [diagrama de flujo](https://www.ibm.com/developerworks/rational/library/3101.html)
5. Al momento de ver su interacción puedes ver que métodos expondrás en cada objeto.

### Object-oriented programming (OOP)

Paradigma de programación basado en objetos, que busca obtener beneficios de modularidad y reusabilidad.

- Bottom-up approach. Primero se diseñan los objetos, y de ahí vas definiendo como interactúan.
- Se enfoca en datos, y métodos que dictan como se interactúa con los datos.
- Se interactúa entre los objetos con funciones (Apis)
- Se reutiliza el diseño ya que se puede reutilizar alguna clase

Los elementos básicos de OOP:

#### Objeto

Entidad que tiene un &quot;estado&quot; y un comportamiento, ie: todo

#### Class

Témplate para objetos, que provee un &quot;estado&quot; inicial (que puede ser modificado al momento de la creación).

#### Polymorphism

Buena descripción en esta [respuesta](http://stackoverflow.com/a/1031385/957979), en el que se menciona el ejemplo de _Shape_ que establece que se reaccionara a la llamada de _draw( )_, el cual puede ser heredado por muchas classes las cuales van a reaccionar de manera &quot;diferente&quot; a lo que Shape establezca. Con esto obtienes &quot;polimorfismo&quot; Ya que tú puedes trabajar sobre el objeto Shape, y obtener diferentes resultados.

Ahora un ejemplo más &quot;complejo&quot; seria sobre _integers vs float_, ya que estos pueden cambiar, si los sumamos obtenemos el uno o el otro (con casting) pero a final de cuentas podemos hacer que uno cambie de forma, ya que puede ser representado de otra manera. A final de cuentas todos son _Números_

#### Abstraction

Manera de ofrecer una &quot;sobre simplificada&quot; descripción de un objeto, ie [mouse](http://softwareengineering.stackexchange.com/questions/16070/what-is-abstraction), este puede tener &quot;n&quot; numero de features, que si ergonómico, que si wireless, que si de gamer, etc. Pero la abstracción del mouse es &quot;pointer que da right y left clicks&quot;. Para tu implementar un mouse tienes que mínimo hacer eso, si haces mas eso ya es un &quot;extra&quot;.

Y está pensada para eso, para facilitar la implementación de comportamientos ya definidos.

#### Encapsulamiento

Puedes decidir quien tiene o no acceso a tus &quot;datos&quot;, lo defines mediante Access Modifiers (private, default, protected, public)

Nota: hay otros modifiers pero estos no son de acceso ie static, abstract, synchronized, native, volatile, transient etc.

Con el encapsulamiento buscamos ofrecer una &quot;API&quot; de lo que te interesa únicamente, nota puedes lograr diferentes &quot;apis&quot; si los interesados perteneces o no a tu package/herencia, etc.

#### Modularidad

Zapatero a sus zapatos. Los sistemas están compuestos de diferentes componentes (módulos). Con este se busca obtener que cada clase tenga una funcionalidad definida, así puedes identificar y re trabajar partes específicas del sistema si es necesario.

#### Herencia

Este es el concepto más sencillo de entender, y el clásico ejemplo de [Perro —&gt; Animal](http://stackoverflow.com/a/575242/957979) es bastante común.


Ok, herencia para todo, bueno aquí es donde entra la discusión de [herencia vs composición](http://www.javaworld.com/article/2076814/core-java/inheritance-versus-composition--which-one-should-you-choose-.html), yo de este entiendo que:

OOP (y todo paradigma según yo) se enfoca en establecer las relaciones entre clases y datos.

**Herencia** es extender o implementar una interface/class

![Herencia](/images/post_ooa/graphic1.jpg)

Cuando estableces la relación con herencia, tomas ventaja de la posibilidad de Dynamic Binding\* y Polimorfismo. Las ventajas de esto es que es más fácil hacer cambios en código, digamos que utilizas en el código Fruit pero requieres otro comportamiento con la misma api, lo puedes hacer generando otra class que extienda/implemente Fruit.

Suena como la solución ideal verdad, pero que tal si ahora queremos cambiar Fruit, esto afecta no solo a quien lo implementa si no a quien lo utiliza también. Por eso se dice en herencia las interfaces son &quot; **frágiles**&quot;, son muy útiles si tienes muy bien definida la interacción que tendrán tus objetos, y en la &quot;vida real&quot; los ejemplos pueden ser más complejos que Fruit &lt;— Apple.

```
class Peel {
    private int peelCount;
    public Peel(int peelCount) {
        this.peelCount = peelCount;
    }

    public int getPeelCount() {
        return peelCount;
    }
    //...
}

class Fruit {
    // Return a Peel object that
    // results from the peeling activity.
    public Peel peel() {
        System.out.println(&quot;Peeling is appealing.&quot;);
        return new Peel(1);
    }
}


// Apple still compiles and works fine
class Apple extends Fruit {}
// This old implementation of Example1
// is broken and won&#39;t compile.

class Example1 {
    public static void main(String[] args) {
        Apple apple = new Apple();
        int pieces = apple.peel();
    }
}
```

**Composición** es utilizar variables de instancia como referencia a los objetos.

![Composicion](/images/post_ooa/composicion.jpg)

Ahora la alternativa que es composition, sería crear una instancia de Fruit en Apple, esto suena un poco contradictorio pensando en términos de &quot;Objetos&quot;, pero los beneficios es que ahora tu puedes inyectar el comportamiento que deseas al llamar un método.

```
class Fruit {
    // Return int number of pieces of peel that
    // resulted from the peeling activity.
    public int peel() {
        System.out.println(&quot;Peeling is appealing.&quot;);
        return 1;
    }
}

class Apple {
    private Fruit fruit = new Fruit();
    public int peel() {
        return fruit.peel();
    }
}

class Example2 {
    public static void main(String[] args) {
        Apple apple = new Apple();
        int pieces = apple.peel();
    }
}
```


Así tu encapsulas el método, te conviertes en el &quot;front-end&quot; de la llamada al método. A este approach normalmente se le conoce como &quot;forwarding&quot; o &quot;delegating&quot;.

El **Composition** approach, ofrece mejor encapsulamiento que por herencia, ya que un cambio en el &quot;back-end&quot; no necesariamente rompe el codigo del &quot;front-end&quot;. Por ejemplo, si cambiamos en return type de Fruit.peel(), este no afecta a los que consumen Apple.peel(), ya que Apple tiene la responsabilidad de &quot;castear&quot; el resultado de Apple.peel(), a lo que esperan sus clientes.

Algunas facts de Composition:

1. Facilita los cambios en &quot;Perent&quot; classes, por lo ya explicado.
2. Facilita ofrecer diferentes APIs, que a final dependen del mismo padre, ya que Apple, y Orange, pueden exponer peel() de manera diferente incluso regresando objetos diferentes.
3. Permite retrasar o incluso no inicializar objetos no requeridos, ie, si no tendrás funcionalidad de Fruit, podrias no instanciarlo.
4. Suele depender del JVM el performance, ya que la instanciacion de objetos puede ser &quot;costosa&quot;
5. &quot;Chaining&quot; la implementacion es igual de fácil, ya que instanciando el objeto obtienes acceso a la funcionalidad.

Nota:

Asegúrate que el is-a y has-a aplica, y tener bien identificada la API de tus objetos.

No utilizar herencia solo para obtener reutilización de código.

No utilizar herencia solo para obtener polimorfismo

*Dynamic Binding: JVM va a decidir que método invocar dependiendo en la implementación o clase del objeto. _

### Coopling y Cohesion.

Siempre escuchamos que debemos de buscar high coopling y loose cohesion. o era al revés?

 Estos conceptos se refieren al diseñar tus objetos buscar hacer que sean lo mas &quot;concretos&quot; posibles. Y esto se puede alcanzar con:

**Loose Coopling** ; La relación entre cliente y cajero.

Se comunican por un canal establecido.

La interacción es definida por el contexto, ie, no puedes pedirle un carro al cajero de la frutería.

Cualquiera puede ser remplazado sin romper la &quot;lógica&quot; establecida.

**High Cohesion** ; Una tienda especializada en café.

Solo vende café, por lo cual tiene (se supone) una especialización en eso, por lo cual si quieres comprar café, será una mejor alternativa que el &quot;súper&quot; en cuanto a calidad o cuando menos la interacción con la tienda.

**Composition**

Ya se habló sobre Composition en una comparación con herencia. La composición es la forma de referirnos a las relaciones entre clases, hay 3 tipos **Association, Aggregation, Composition**.

![Composicion](/images/post_ooa/composition2.jpg)

Como el diagrama lo muestra, no son independientes, uno es el otro + algo más.  Aggregation es un caso especial de Association, Composition es un caso especial de Aggregation.

**Asociacion** , one-to-one, one-to-many, many-to-one, many-to-many son ejemplos de asociación. Este es el punto general ósea el termino genérico para la relación entre 2 objetos.

UML:

![Asociacion](/images/post_ooa/asociation.png)

**Agregacion**. Agregacion que satisface la condición &quot;has-a&quot; y la dirección es clara, en la cual es claro cual objeto tiene el otro objeto, ie carro y estéreo.

UML

![Agregacion](/images/post_ooa/aggregation.png)

Composition. Es una Agregación restrictiva, Cuando un objeto contiene otro objeto y el objeto contenido (child) no puede existir sin el padre, ejemplo, Estudiante y Clase, La clase contiene más estudiantes, y a su vez el estudiante sin ninguna clase, no es estudiante.

UML

![Agregacion](/images/post_ooa/class-composition.png)

**Abstraction**.

Abstracción es especificar el Framework, ocultando la implementación, da la guía (blueprint) a seguir para la implementación.

**Generalización** , es cuando se cumple la condición &quot;is-a&quot;, desde la especialización a la generalización, ie: Student is-a Person, es general dentro del sistema ya que varios módulos pueden tener la misma generalización ie Student, Teacher su función es diferente pero ambos son personas.

En java lo veríamos cuando una clase extiende otra.

![Generalización](/images/post_ooa/class-generalizaion-separate.png)


**Realization** , relación entre la interfaz y objeto que implemento los detalles. En java es la relación entre una interfaz y una clase concreta.

![Realization](/images/post_ooa/class-interface-realization-ball.png)

**Dependency** , cuando el cambio de estructura o comportamiento de una clase afecta a otra clase, y no necesariamente alrevez, ie. Shape and Drawer.

![Dependencia](/images/post_ooa/class-dependency.png)

Esta definición es muy ambigua, en código las dependencias se pueden ver desde el cómo nombras una variable. si cambias el nombre va a afectar otras clases? esas clases (la extiendan o no) son dependientes de tu clase.

#### Mis best praciteces de OOP: Very opinionated, probably wrong.

Al diseñar una clase:

- Que la interfaz sea lo mínima posible, que tenga los menos métodos posibles.
- Oculta la implementación, ósea como hace su &quot;business&quot; realmente no le debería de interesar a quien consume tu objeto.
- Constructores, que tu constructor genere el objeto en un estado saludable, si se requiere x o y para que este funcione, pídelos como dependencias o no permitas la construcción del mismo.
- Si va a fallar que no se entere quien no debe de enterarse, maneja errores internos, quiza tengas que arrojar errores, pero de preferencia que sean descriptivos.
- Los objetos son creados con la finalidad de cooperar, trata de ver la utilización.
- Diseña pensando en la reutilización (por herencia o composición) pero probablemente tu lógica pueda ser reutilizada.
- Abstrae tanto como sea posible, incluso si puedes &quot;behavior&quot; y podrías inyectar el behavior con un strategy pattern, pero bueno eso ya es otro tema.
- Si es una estructura de datos, piensa en si será copiado o clonado, e implementa esa funcionalidad.
- Muy probablemente sea utilizado en las súper estructuras de datos HashSet, o HashMap, implementa un _hashCode, equals_ que hagan sentido a tu lógica.
- Piensa en cachos de lógica &quot;testeables&quot; y abstraelos, así to TDD será mucho más sencillo.
- Piensa si y como será persistido, _Serializable, Entity, etc._ buscamos que como lo &quot;guardamos&quot; lo recuperemos.



## Object–Oriented Design (OOD)

Implementar el modelo conceptual diseñado en OOA, En esta etapa es orientar el análisis sobre una tecnología,

Resultando en una detallada descripción de como el sistema será construido en la tecnología seleccionada.

- Implementación de métodos específicos, ie: estructura de datos internas y algoritmos
- Implementación de control de acceso y otros tipos
- Implementación de asociaciones.

Se dice que todo OOD tiene que tener las características de [S.O.L.I.D.](https://scotch.io/bar-talk/s-o-l-i-d-the-first-five-principles-of-object-oriented-design) si, justo como las base de datos.

- S – Single-responsiblity principle
- O – Open-closed principle
- L – Liskov substitution principle
- I – Interface segregation principle
- D – Dependency Inversion Principle



### Single-responsibility Principle

- &quot;A class should have one and only one reason to change, meaning that a class should have only one job.&quot;

Suena sencillo verdad, suele llegar a complicarse. Por ejemplo:

Tenemos nuestro Shape, y tenemos muchas implementaciones de Shape, ie Circle, Square, etc.

Ahora queremos calcular el área, para esto creamos nuestro &quot;AreaCalculator&quot; el cual tendrá el metodo calcular:

```
class Circle {
    public $radius;
    public function __construct($radius) {
        $this->radius = $radius;
    }
}

class Square {
    public $length;
    public function __construct($length) {
        $this->length = $length;
    }
}

… 

class AreaCalculator {
    protected $shapes;
    public function __construct($shapes = array()) {
        $this->shapes = $shapes;
    }

    public function sum() {
        // logic to sum the areas
    }

    public function output() {
        return implode('', array(
            "<h1>",
                "Sum of the areas of provided shapes: ",
                $this->sum(),
            "</h1>"
        ));
    }
}
```
Codigo en PHP porque "to lazy", el ejemplo completo con la explicacion esta en “[aqui](https://scotch.io/bar-talk/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)"




Perfecto, verdad, pero que pasa si queremos el resultado en algo que no sea &quot;html&quot;, si lo queremos en Json, bueno para eso tendríamos que modificar o sobrecargar el código en AreaCalculator, con algo que realmente no es su responsabilidad, es como cuando te ponen a diseniar y tú dices &quot;no yo soy solo desarrollador&quot; o no lo haces o lo haces muy mal verdad.

Para corregir esto, podríamos crear otra clase que su objetivo es &quot;generar outputs&quot; _SumCalculatorOutputter _

```
$shapes = array(
    new Circle(2),
    new Square(5),
    new Square(6)
);
```

$areas = new AreaCalculator($shapes);

$output = new SumCalculatorOutputter($areas);

echo $output-&gt;JSON();

echo $output-&gt;HAML();

echo $output-&gt;HTML();

echo $output-&gt;JADE();

Así, nos aseguramos que cada uno se especialice en lo que ellos desean.

**Open-closed Principle**.

- &quot;Objects or entities should be open for extension, but closed for modification.&quot;

Esto con el ejemplo que tenemos de **AreaCalculator** , tenemos declarado el &quot;sum&quot;. El cual para satisfacer todos los diferentes tipos de shapes, tendríamos que tener muchos ifs.

Una manera que se podría solucionar esto es, calculando el área independientemente por shape, que cada shape implemente un &quot;getArea&quot; y así el AreaCalculator, llamaría el área de cada uno.

```
interface ShapeInterface {
    public function area();
}

class Circle implements ShapeInterface {
    public $radius;
    public function __construct($radius) {
        $this-&gt;radius = $radius;
    }
    public function area() {
        return pi() \* pow($this-&gt;radius, 2);
    }
}

…

public function sum() {
    foreach($this-&gt;shapes as $shape) {
        if(is\_a($shape, &#39;ShapeInterface&#39;)) {
            $area[] = $shape-&gt;area();
            continue;
        }
        throw new AreaCalculatorInvalidShapeException;
    }
    return array\_sum($area);
}
```


Liskov substitution principle

- &quot;Let **q(x)** be a property provable about objects of  **x**  of type  **T**. Then **q(y)** should be provable for objects **y**  of type  **S**  where  **S**  is a subtype of  **T**.&quot;

Wait,.. what!?!?

Osea que toda subclase, debe de poder ser substituida por su clase parent!

En nuestro ejemplo (copiado) es como si Quisiéramos implementar un VolumeCalculator, el cual extienda el AreaCalculator, y implementamos el metodo &quot;sum&quot; de manera diferente, esto romperia la regla de substitucion.

```
class VolumeCalculator extends AreaCalulator {
    public function __construct($shapes = array()) {
        parent::__construct($shapes);
    }

    public function sum() {
        // logic to calculate the volumes and then return and array of output
        return array($summedData);
    }
}
```

En el ejemplo descrito, este no funcionaria, ya que no cumple con la misma &quot;firma&quot; que la clase padre exige, nota esto en un static type language, como Java no funcionaría ya que arrojaría un error de compilación, o en otras palabras el IDE se quejaría.

### Interface segregation principle

- &quot;A client should never be forced to implement an interface that it doesn&#39;t use or clients shouldn&#39;t be forced to depend on methods they do not use.&quot;

No porque seria &quot;nice to have&quot; hay que hacer que la clase que implemente la interfaz tenga que implementar metodos que no le son de interes.

```
interface ShapeInterface {
    public function area();
    public function volume();
}
```

Esta sería una interfaz mal planeada, porque sabemos que Square, no tiene volumen, y estaríamos obligándolo a regresar un valor, e incluso sus usuarios se confundirían al ver eso.

Para solucionar algo asi, se invento la multiple herencia.
```
interface ShapeInterface {
    public function area();
}
...
interface SolidShapeInterface {
    public function volume();
}
...
class Cuboid implements ShapeInterface, SolidShapeInterface {
    public function area() {
        // calculate the surface area of the cuboid
    }
    public function volume() {
        // calculate the volume of the cuboid
    }
}
```

Para cuando requieres declarar una firma que ambas interfaces la &quot;implementen&quot; se puede utilizar otra interfaz que ambas clases la implementen. Esto se puede complicar tanto como tu quieras... A veces, es mejor agregar composición.

### Dependency Inversion principle

- &quot;Entities must depend on abstractions not on concretions. It states that the high level module must not depend on the low level module, but they should depend on abstractions&quot;

Dependency Injection, Para los que están familiarizados con Spring, es facil de entender o Angular, o etc. En si esto consiste en que cuando tengas &quot;dependencias&quot; quitarle la responsabilidad a tu clase de crearla, ofrecer un &quot;setter&quot; para inyectar la dependencia concreta.

Un ejemplo seria como inyectamos la conexión a la base de datos.
```
class MySQLConnection implements DBConnectionInterface {
    public function connect() {
        return &quot;Database connection&quot;;
    }
}

class PasswordReminder {
    private $dbConnection;
    public function __construct(DBConnectionInterface $dbConnection) {
        $this-&gt;dbConnection = $dbConnection;
    }
}
```


** Términos en inglés, porque así los puedes googlear mas fácil._