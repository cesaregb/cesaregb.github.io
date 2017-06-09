---
layout: post
title: Reactive Programing en Java (Español)
author: Cesar Gonzalez
---

# Reactive Programing en Java (Español)

**Que es Reactive Programing?**

Otro paradigma, asi como[ Imperative
programming](https://en.wikipedia.org/wiki/Imperative_programming) o [Functional
programming](https://en.wikipedia.org/wiki/Functional_programming). [Reactive
Programing](https://en.wikipedia.org/wiki/Reactive_programming) (RP), es otra
metodología, con la cual puedes llegar a soluciones a los mismos problemas,
algunas veces con una mejor utilización de recursos.

**Mejor utilizacion de recursos?**

Si, todo surgió a raiz de node.js el porque se podia llegar a buen performance
con un “single thread” approach.

Con RP podemos obtener

* *Mejor abstracción a eventos*
* *Abstrae y “mejora” manejo de threads, concurrency y sincronización.*

Pero como todo en programación, hay que guiarnos por los básicos consejos de los
experimentados.

> - “La app que tiene menos problemas es la que no existe.”

Tiene que haber razones para cambiar este modelo, beneficios reales, porque si
te alejaras del “long-running” transactions no garantiza que obtendrás una
mejora ya que podrías terminar dependiendo de un “blocking resource” como la
Base de datos.

El imperative style es funcional y sencillo de mantener ya que puedes pensar de
manera sincrona, te ayuda con las excepciones, ya que las puedes manejar tal
cual vienen, y hay muchas maneras de optimizar antes de cambiar completamente el
paradigma de la aplicación.

**Que es reactive programing.**

Es programar con el observable pattern en mente, asi disminuir el tiempo que el
sistema espera recursos.

A toda accion le pertenece una reacción, es adaptar tu aplicación para el que
hace la acción notifique al que tiene que reaccionar.

- “Pero ya habia escuchado del observable pattern cual es la diferencia con RP?”

Reactive programing es mas que solo el observable patter, hay un
[manifesto](http://www.reactivemanifesto.org/) en el cual se va a detalle que se
cubre con RP

Mi manera de ver RP, y antes de aplicarla tenemos que pensar en la comunicación
entre los interesados.

Como en toda relacion, la comunicacion tiene que ser mutua, si no, eso no va a
funcionar, ie:

Hay varios Uber por tu casa (observers), y cuando solicitas el servicio
(observable), alguno de ellos reacciona a tu acción.

Si la comunicación se rompiera, ya sea que tu no notificaras que requieres el
servicio, jamas llegarían, y a su vez si notificaras y ellos no atendieran el
request, igual nunca llegaría, se require la participación de los 2.

RP esta basado en streams de eventos.

Stream es como el radio, el flujo de ondas va, tu decides si sintonizarlo o no,
(subscribirte). Y una vez escuchandolo, tu decides que hacer con el (cantar)

Por lo cual stram es un flujo, vamos a programar esperando un flujo y tomando
desiciones con ese flujo.

Ejemplos de como detectar si es util o no DP

* A veces es mas rapido ejecutar jobs syncronos si la info esta en cache evitas el
arrancar un job de la nada.
* Lo mas util de RP es cuando ocupas hacer funciones compuestas (filter, transfor,
combine etc.) Manipulacion de streamings.

Normalmente RP se representa con “diagrama de dulces” ie:

**Ahora sobre Java.**

**Java SPEC**

En java, la spec es: (formada por spring, netflix, twitter, etc etc)

[https://github.com/reactive-streams/reactive-streams-jvm](https://github.com/reactive-streams/reactive-streams-jvm)

**Soluciones Existentes**

Algunas de las librerias mas “fuertes” que existen, son

* Java8 [
[CompletableFuture](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html)
]
* Java9 [[Flow](https://community.oracle.com/docs/DOC-1006738)]
* Netflix [RxJava1 y [RxJava2](https://github.com/ReactiveX/RxJava)]

[https://www.youtube.com/watch?v=_t06LRX0DV0&t=122s](https://www.youtube.com/watch?v=_t06LRX0DV0&t=122s)

[https://www.youtube.com/watch?v=htIXKI5gOQU](https://www.youtube.com/watch?v=htIXKI5gOQU)

[https://spring.io/blog/2016/02/16/reactor-2-5-a-second-generation-reactive-foundation-for-the-jvm](https://spring.io/blog/2016/02/16/reactor-2-5-a-second-generation-reactive-foundation-for-the-jvm)

* Spring [[Reactor Core](https://github.com/reactor/reactor-core) Integrado
apartir de Spring 5]

[https://qconnewyork.com/system/files/presentation-slides/reactive_programming_for_java_developers.pdf](https://qconnewyork.com/system/files/presentation-slides/reactive_programming_for_java_developers.pdf)

*****

El
[CompletableFuture](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html)
es nuevo en Java8 es un “superset” del
[Future](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Future.html).

Porque future seguia siendo blocking a final de cuentas tenias que esperar el
“.get()”. para poder trabajar sobre ese resultado.

El Furure, te da un promise, que se llena cuando tiene “todo” el resultado. Es
blocking porque el esperar por el get() bloquea la asincronizidad.

    public static class MyCallable implements Callable<Integer> {
       
       public Integer call() throws Exception {
         Thread.sleep(1000);
         return 1;
       }
    }

    public static void main(String[] args) throws Exception{
       ExecutorService exec = Executors.newSingleThreadExecutor();
       Future<Integer> f = exec.submit(new MyCallable());
       System.out.println(f.isDone()); //False
       System.out.println(f.get()); //Waits until the task is done, then prints 1
    }

Completable Future son evolución del Future, que te permite, hacer chaining de
tareas. y un Future pero non-blocking.

Primero, la firma de CompletableFuture es:

    CompletableFuture.supplyAsync(this::findReceiver)
      .exceptionally(throwable)
      .thenApply(this::sendMsg)
      .thenAccept(this::notify);

El *“supplyAsync()”* Acepta un Sipplier, y regresa un CompletableFuture
representando la TAREA.

Nota: Esto se ejecuta con threads con un “*java.util.concurrent.ForkJoinPool
(public static ForkJoinPool commonPool())*”

Pero podríamos configurar la ejecución inyectando un
java.util.concurrent.Executor

*this::findReceiver ->* Es un
[Supplier](https://docs.oracle.com/javase/8/docs/api/java/util/function/Supplier.html).
Interfaz declarada para que no acepte nada y regresa un resultado.

Para hacer chaining the multiples Completable Future:

    CompletableFuture<Double> completableFuture2
         = CompletableFuture.supplyAsync(TaskSupplier::getSomeArbitraryDouble);

    CompletableFuture<Double> completableFuture3
         = CompletableFuture.supplyAsync(TaskSupplier::getAnotherArbitraryDouble);

    // Nest or join CFs 
    CompletableFuture<Void> completableFutureForAcptEither
         = completableFuture2.acceptEitherAsync(completableFuture3, (val) -> System.out.println("val: " + val));

Completable Future esta basado en la interfaz CompletionStage esta intefaz tiene
la finalidad de abstraer bloques de trabajo, estas pueden o no ser implementadas
asyncronas.

Esta esta diseniada para poder ejecutar en “pipe” el resultado de una ejecucion
lo puedes disparar una siguiente tarea.

Parte importante de RP es “error propagation”, mas cuando se ejecuta en threads,
propagar o saber que hacer con ese error no es “sencillo” para eso se utiliza el
**.exceptionally( )** y ya como owner del Completable Future decides que hacer.

Completable Future provee varios metodos bien documentados en la
[API](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html).
Para hacer chaining de tasks, hay desde agrupar, separar, procesar, etc.

### [Cesar Gonzalez](https://medium.com/@cesaregb)

....


