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

## Operadores

### _map()_

```java
Observable.just("C#", "Java", "Python").map(String::length)
    .subscribe(System.out::println);
```

### _filter()_

```java
Observable.just("C#", "Java", "Python").filter(e -> e.length)
    .subscribe(System.out::println);
```

### _distinct()_

```java
Observable.just("C#", "Java", "Python").map(String::length).distinct()
    .subscribe(System.out::println);
```

### _take()_

```java
Observable.just("C#", "Java", "Python").take(2)
    .subscribe(System.out::println);
```

### _takeWhile()_

Algunos nombres de operadores varían en su aplicación en el lenguaje, pero el funcionamiento sigue siendo el mismo. En el caso de _takeWhile()_:

```java
Observable.just("C#", "Java", "Python").takeUntil(l -> l.length < 5)
    .subscribe(System.out::println);
```

### _count()_

```java
Observable.just("C#", "Java", "Python").count()
    .subscribe(System.out::println);
```

### _toList()_

```java
Observable.just("C#", "Java", "Python").toList()
    .subscribe(System.out::println);
```

---
#### [Volver al inicio][back]

[reactiveprogramming]: http://reactivex.io/
[speakingoperators]: https://medium.freecodecamp.org/rx-if-the-operators-could-speak-58567c4618f1

[back]: ../README.md