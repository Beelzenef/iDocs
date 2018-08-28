# Programación reactiva

[_Reactive programming_ o _Rx_][reactiveprogramming] es un paradigma de programación con independencia de lenguaje que promueve el tratamiento igualitario de eventos y de datos.

Todo este tratamiento se efectúa con la concepción de flujos, todo en la programación reactiva es un flujo... y de ahí surgen los datos, ya sean datos sin más o también eventos. Para mayor claridad, un post en [Medium explica los operadores como si estos pudieran hablar][speakingoperators]. Una lectura tan entretenida como instructiva.

```java
List<Integer> numberList = Arrays.asList(1, 2, 3, 4);
Observable list = Observable.fromIterable(numberList);
```

Para los ejemplos que siguen, usaremos el siguiente Observable:

```java
Observable langs = Observable.just("C#", "Java", "Python");
```

Rx crea una nueva forma de convertir a los Productores en elementos activos y a los Consumidores en elementos pasivos. Los Observables producen emisiones y los consumidores reaccionan ante estas nuevas emisiones.

¿Cuál es la diferencia entre un Observable y una función? Mientras las valores de las funciones dependen de las veces que son invocadas, los Observables pueden devolver, emitir, tantos valores como sean necesarios. Pueden emitir desde 0 a (potencialmente) infinitos valores.

Los Observables son síncronos, pero pueden actuar de forma asíncrona.

Las funciones pueden devolver un solo valor de forma síncrona.
Los Observables pueden devolver una cantidad indeterminada de valores de forma síncrona o asíncrona.

Un Observador, en cambio, es un consumidor que recoge los valores emitidos por un Observable. Son objetos con tres _callbacks_, uno por cada tipo de notificación que un Observable puede entregar:

* `onNext()`, para el siguiente valor emitido
* `onError()`, cuando hay un error en el Observable
* `onComplete()`, cuando la emisión finaliza, el Observable ha completado su operación.

No necesitas proveer de todos los _callbacks_, el Observable continuará con su ciclo de vida de forma natural sin que puedas reaccionar a aquellos que omitas.

## Operadores

### _map()_

Recorre todos los elementos del flujo y los transforma según la función llamada en el operador.

```java
Observable.just("C#", "Java", "Python").map(String::length)
    .subscribe(System.out::println);
```

### _filter()_

Filtra y envía los elementos del flujo que cumplen con ese filtro.

```java
Observable.just("C#", "Java", "Python").filter(e -> e.length)
    .subscribe(System.out::println);
```

### _distinct()_

Se detectan los elementos emitidos que resultan diferentes. Si se les aplica un `map()` como en el ejemplo, emite solo aquellas longitudes de cadena de texto que sean diferentes del resto, que no se repitan.

```java
Observable.just("C#", "Java", "Python").map(String::length).distinct()
    .subscribe(System.out::println);
```

### _take()_

Recibe un número entero para recoger el item que se encuentra en ese índice.

```java
Observable.just("C#", "Java", "Python").take(2)
    .subscribe(System.out::println);
```

### _takeWhile()_

Algunos nombres de operadores varían en su aplicación en el lenguaje, pero el funcionamiento sigue siendo el mismo. En el caso de `takeWhile()`:

```java
Observable.just("C#", "Java", "Python").takeUntil(l -> l.length < 5)
    .subscribe(System.out::println);
```

### _count()_

Cuenta los elementos que el flujo emite hasta su llamada.

```java
Observable.just("C#", "Java", "Python").count()
    .subscribe(System.out::println);
```

### _toList()_

Convierte el flujo en una lista, que podemos guardar y operar con ella.

```java
Observable.just("C#", "Java", "Python").toList()
    .subscribe(System.out::println);
```

### _combineLatest()_

Combina los flujos que se le especifican, en una lista de Observables, en un combinador que puedes recoger para operar con él. Puede ser una nueva instancia, elementos individuales...

```java
Observable.combineLatest(Obs1, Obs2, ..., combiner, accion -> { combiner.operaciones(); }
    .doOnNext(() -> resultadoDeOperaciones())
        .subscribe();
```

### _merge()_

Mezcla dos o más Observables, dos flujos, hasta convertirlos en uno solo para poder realizar operaciones con el mismo.

```java
Observable.merge(Obs1, Obs2, ...)
    .doOnNext(() -> operaciones())
        .subscribe();
```

### _zip()_

Combina Observables del mismo modo que hace una cremallera al ser usada. Coge por parejas o tríos... correspondientes en un objeto combinado

```java
Observable.merge(Obs1, Obs2, ..., (o1, o2))
    .doOnNext(() -> operacionesCombinadas())
        .subscribe();
```

## Tipos de Observables

### _Subject_

Es un tipo especial de Observable que permite multicasting de varios Observables, semejante a los múltiples _listeners_ que puede tener un _widget_ en cualquier librería o lenguaje. Mantiene un registro de varios _listeners_ para emitir los diferentes eventos que sucedan.

Cada `Subject` es un Observable, con todos los métodos a su disposición como en cualquier otro. Para proveer a un Subject de un nuevo valor, invocarás a `next(newValue)`.

Puedes utilizar un `Subject` como argumento para `subscribe()`de cualquier Observable, siendo un Observable esta misma Clase.

[Sigue el enlace para leer más.](http://reactivex.io/rxjs/manual/overview.html#subject)

### _BehaviourSubject_

Una especialización de `Subject` que sirve para representar valores en el tiempo. Descrito en la página de RxJs, si `Subject` es el flujo de eventos para cumplir años, `BehaviourSubject` es el flujo de eventos que refleja la edad de una persona. 

Puede ser de un tipo concreto. En RxJs, el constructor de la Clase le ofrece el valor inicial.

```javascript
var subject = new Rx.BehaviorSubject(0); 
```

Este Observable recuerda el último valor emitido, enviandolo a cualquier Observable que se suscriba, aunque este valor ya se haya emitido en el pasado.

[Sigue el enlace para leer más.](http://reactivex.io/rxjs/manual/overview.html#behaviorsubject)

### _ReplaySubject_

Otra especialización de `Subject`, similar a `BehaviourSubject` porque puede emitir valores "antiguos" a nuevos suscriptores, pero también puede "grabar" una parte de la ejecución del Observable.

```javascript
var subject = new Rx.ReplaySubject(3);
```

En el siguiente código, creamos un _ReplaySubject_ que recordará los tres últimos valores, que irá reproduciendo a medida que lleguen (invocaciones a `next()`).

```javascript
var subject = new Rx.ReplaySubject(3);


subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});

subject.next(1);
subject.next(2);
subject.next(3);
subject.next(4);

subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

subject.next(5);
```

Y el resultado a esperar:

```
observerA: 1
observerA: 2
observerA: 3
observerA: 4
observerB: 2
observerB: 3
observerB: 4
observerA: 5
observerB: 5
```

Lo que ocurre: se muestran en Observable A los valores conforme son emitidos. En algún momento, se suscribe el Observable B, y se reproducen los tres valores que hemos indicado que debe recordar hacia atrás, en el orden en el que se emitieron. Una vez emitidos, se emiten los valores nuevos que llegan... sin más valores que emitir.

[Sigue el enlace para leer más.](http://reactivex.io/rxjs/manual/overview.html#replaysubject)

---
#### [Volver al inicio][back]

[reactiveprogramming]: http://reactivex.io/
[speakingoperators]: https://medium.freecodecamp.org/rx-if-the-operators-could-speak-58567c4618f1

[back]: ../README.md
