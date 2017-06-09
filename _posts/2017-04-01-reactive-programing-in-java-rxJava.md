---
layout: post
title: Reactive Programing en Java rxJava (Español)
author: Cesar Gonzalez
---

# Reactive Programing en Java (Español) RxJava

Esta parte esta basada casi completamente en una excelente guia que esta en:

[https://github.com/Froussios/Intro-To-RxJava](https://github.com/Froussios/Intro-To-RxJava)

Probablemente sea mucho mas entendible esta guia que lo que yo escribo.

[Rx Java](https://github.com/ReactiveX/RxJava) y/o [RxJava 2
](http://reactivex.io/RxJava/2.x/javadoc/)([Que
cambio](https://github.com/ReactiveX/RxJava/wiki/What's-different-in-2.0))

Rx esta basado sobre
[Observable](http://reactivex.io/RxJava/2.x/javadoc/io/reactivex/Observable.html)
representa el recurso o “hotspot” de que se brindan los recursos.

La utilización de
[Observer](http://reactivex.io/RxJava/2.x/javadoc/io/reactivex/Observer.html) Es
la herramienta utilizada para consumir el observable.

**Nota***: A lo largo de esta guia me refiero a “Observable”, en todo momento
este puede ser cambiado por la implementación que soporta backpressure de RxJava
*[Flowable](http://reactivex.io/RxJava/2.x/javadoc/io/reactivex/Flowable.html)*.
Al final del post, hago mención del porque uno vs el otro.*

Siguiendo la implementación de observer design pattern:

* Observer subscribe a Observable.
* Observer reacciona a las acciones emitidas por el Observable.

**Observable (El Flowable que mencione en la nota)**

El Observable emite 3 tipos de eventos.

* valores .onNext( )
* finalización .onComplete( )
* error .onError( )

El observable emite, las llamadas mencionadas, para esto la interfaz Observer,
esta listo para recibir estas con la interfaz:

    interface Observer<T> {
      void onCompleted();
      void onError(java.lang.Throwable e);
      void onNext(T t);
    }

La interacción normal con RP seria:

    def myOnNext = { item -> /* do something useful with item */ };
    def myError = { throwable -> /* react sensibly to a failed call */};
    def myComplete = { /* clean up after the final response */ };
    def myObservable = someMethod(itsParameters);
    myObservable.subscribe(myOnNext, myError, myComplete);
    // go on about my business

Donde myOnNext, myError, myComplete serian partes del Observer.

Como crear un Observable:

* Observable.create();
* Observable.just(Object);
* Observable.fromArray();
* Observable.fromIterable();
* Observable.fromCallable();
* Observable.fromPublisher();

    Observable<JsonElement> todoObservable = Observable.create(emitter -> {
       try {
         for (String type : types){
            JsonElement data = fetchData(type);
            if (data == null){
              throw new Exception("no jalo la llamadita");
            }
            emitter.onNext(data);
         }
         emitter.onComplete();
       } catch (Exception e) {
         emitter.onError(e);
       }
    });

**Observer o listener**

Hay varias implementaciones de Observer, dependiendo de que se busque hacer,
pero lo mas común es:

    DisposableObserver<String> s = values
    .onErrorResumeNext(Observable.just("hard"))
    .subscribeWith(new DisposableObserver<String>() {
       
       public void onNext(String s) {
         System.out.println(s);
       }
       
       public void onError(Throwable e) {
         System.out.println("not reached... ");
       }
       
       public void onComplete() {
         System.out.println("completed");
       }
    });

Un observer tiene la limitación de que no puedes “propagar” el resultado, como
yo lo entiendo es un punto Finito, si vas a post-processar el resultado seria
llamando otra función o algo así, esto puede no ser muy funcional.

**Subjects**

Hay una Extension de Observer que Implementa Observable, esta es el
[Subject](http://reactivex.io/RxJava/2.x/javadoc/io/reactivex/subjects/Subject.html).

Normalmente es utilizado como entry-point en un pipeline de observables. Aqui se
ve como funciona como Observable.

Proxy porque siendo un Observer, puede subscribirse a uno o mas Observables, y
siendo un Observable, puede propagar eventos.

Como se utiliza un Subject:

    // Subject that I want to have as entrypoint.
    PublishSubject<Long> publishSubject = PublishSubject.create();

    // simple observables
    Observable<Long> interval = Observable.interval(100L, TimeUnit.MILLISECONDS);
    Observable<Long> interval2 = Observable.interval(150L, TimeUnit.MILLISECONDS);

    // subject subscribe to my Observables.
    interval.subscribe(publishSubject);
    interval2.subscribe(publishSubject);

    // add a couple of observers.
    publishSubject.subscribe(getObserver("A"));
    publishSubject.subscribe(getObserver("B"));
    interval.subscribe(getObserver("C"));

Hay varios tipos de Subjects, como:

ReplaySubject : Repite eventos emitidos.

BehavioralSubject: etc.

**Subject vs Observable.**

Normalmente como ya se menciono subject es mas orientado a un entrypoint. Cuando
ocupas varias fuentes o algo que funja como ambos, pero normalmente puedes
trabajar con cualquiera de los 2.

**Subscribe and Unsubscribe**

Esta sucede cuando te “un-suscribes” Yep justo como el spam.

El punto de inicio del Observable es el subscribe el cual tiene diferentes
“firmas”

    Subscription subscribe()
    Subscription  subscribe(Action1<? super T> onNext)
    Subscription  subscribe(Action1<? super T> onNext, Action1<java.lang.Throwable> onError)
    Subscription  subscribe(Action1<? super T> onNext, Action1<java.lang.Throwable> onError, Action0 onComplete)
    Subscription  subscribe(Observer<? super T> observer)
    Subscription  subscribe(Subscriber<? super T> subscriber)

Ejemplo de subscripción a un Observer (o subject):

    Subject<Integer, Integer> s = ReplaySubject.create();
    s.subscribe(
       v -> System.out.println(v),
       e -> System.err.println(e));
    s.onNext(0);
    s.onError(new Exception("Oops"));

Aqui no se tiene declarado un Observable como tal, pero las funciones inyectadas
actúan como tal.

Cuando ya no quieres escuchar a _______, como lo callo?

Como Observer, puedes dejar de recibir los emits del Observable, con
unsubscribe, sobre la subscripción.

    Subject<Integer, Integer> values = ReplaySubject.create();
    Subscription subscription = values.subscribe(
      v -> System.out.println(v),
      e -> System.err.println(e),
      () -> System.out.println("Done")
    );
    values.onNext(0);
    values.onNext(1);
    subscription.unsubscribe(); 
    values.onNext(2);

Cuando ya se tiene el stream de datos, que es lo que se requiere hacer con el, o
como vamos a “Reaccionar” con los datos es lo importante, para esto RxJava
ofrece diferentes metodos.

Los mas comunes son Reducers y Aggregators, ósea lo que Javascript presume, ojo,
esto se puede hacer con los [lambdas de Java
8.](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)

**Reducers**

Para toda función de streams, parte importante de esto son los reducers. para
esto funcionan muy similar a como **streams** en java8

Existen varios tipos de reducers:

* filter
* distinct
* distinctUntilChanged
* ignoreElements
* skip
* take
* skipWhile and takeWhile
* skipLast and takeLast

Un ejemplo de la utilización es:

    Observable<Integer> values = Observable.range(0,10);
    Subscription oddNumbers = values
      .filter(v -> v % 2 == 0)
      .subscribe(
        v -> System.out.println(v),
        e -> System.out.println("Error: " + e),
        () -> System.out.println("Completed")
      );

**Aggregation**

Hay diferentes metodos para agregar datos de un stream ie: count, first, last,
single…

Un ejemplo común de una operación de agregación es reduce:

Yo diría que como independientemente como quieras tus datos ya existe una
implementación sobre el stream que te arrojara ese resultado.

Algunos otros comunes que me gustaría mencionar son orientados a agrupación de
datos en “sub-grupos”, puedes llegar a esto con: *toList, toSortedList, toMap,
toMultimap, groupBy.*

Ahora ya agrupe los datos pero me gustaría modificarlo, RxJava tiene los
transformers, creo este diagrama lo explica mejor e lo que yo podría.

Como mensione en mi post anterior, el manejo de **errores** en RP, es
importante, rxJava da la fasilidad de como reaccionar a un error, ofreciendo los
metodos:

    .onErrorReturn( )
    .onErrorResumeNext || .onExceptionResumeNext()
    .retry( )

Streams concurrentes, ahora hay diferentes maneras de reaccionar a eventos que
pasan en 2+ streams.

Entre ellas esta amb, next, switch, etc. un ejemplo de como se utilizaria en
codigo seria:

    Observable.ambArray(
        Observable.timer(100, TimeUnit.MILLISECONDS).map(i->"a...."),
        Observable.timer(50, TimeUnit.MILLISECONDS).map(i->"b...."))
        .subscribe(System.out::println);

**Backpressure**

Por ultimo me gustaria mensiona el termino backpressure. El termino viene de la
presion que existe de un fluido que ejerce para pasar sobre un conducto que no
tiene la capasidad.

Cuando el productor y consumidor trabajan con “tiempos” diferentes.

Con sync programing esto no pasa, ya que es blocking y hasta que no termina A, B
puede empezar.

Un posible remedio seria reducir el input:

Puedes utilizar window y buffer para almacenar información mientras puedes
consultarla.

Pero El Observable tiene la capacidad de permitirle al Observer indicar cuanta
información espera, asi regulando el consumo.

Observable cuando

Observer en onStart( ) mandame de uno nada mas

Obervable oks, regulare mi .onNext( )

Si no perder información es importante, se puede configurar el observer a no
perder información

Pero, **RxJava 2** le da una mejor solución a backpressure

**Observable and Flowable**

Son lo mismo pero orientado a diferentes cargas.

La principal diferencia sobre usar una vs el otro, es la cantidad de datos que
recibirás siendo **10000** el parteaguas.

    Tienes < 10,000 elementos.
    UI Elements
    Flow Syncrono pero no tienes Java8 😔

    Dealing with 10k+
    Files Operations.
    DB (JDBC) Operations
    Network (Streaming) IO

    En general para hacer blocking en non-blocking.



