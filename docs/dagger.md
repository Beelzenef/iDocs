# Dagger

[Dagger][dagger] es un inyector de dependencias, en el sentido de que las Clases dependen de instancias de otras para su funcionamiento. En circunstancias normales, se pasan como parámetros de constructores... pero cuando el proyecto crece, las inicializaciones se agolpan y el código empieza a obscurecerse.

Es por eso que Dagger ofrece un modo para dar a conocer a las Clases las dependencias que necesitan inyectarse (_pun intended_).

Al proceso de dar a conocer las dependencias a una Clase, o instanciar Clases para pasarlas a la dependiente, se le conoce como _build the dependency graph_, o construir el grafo de dependencias.

Para evitar la locura y la desesperación en nuestros proyectos, Dagger 2 ofrece una solución sencilla.

Lo primero es añadir Dagger a nuestro proyecto, ya sea con [Gradle][addgradle] o con [Maven][addmaven].

Después necesitaremos especificar una anotación para cada Clase con la que vaya a operar Dagger. Dagger necesita saber las Clases que va a inyectar en algún momento, y en cada constructor de las mismas añadiremos una anotación para que pueda recogerlo.

```java
public class ClaseInyectable {
    @Inject public ClaseInyectable { }
}
```

Una vez señada esa Clase, crearemos una interfaz que lleva otra anotación que funcionará como intermediaria. Esta interfaz recoge los métodos que proveen de las dependencias a nuestra Clase.

```java
@Component
public interface AppComponent {
    ClaseInyectable getDependencia();
}
```

Con Dagger en nuestro proyecto, se generan automáticamente las Clases que necesitaremos para nuestro proyecto.

```java
public class Application {
    public static void main(String args[]) {
        AppComponent component = DaggerAppComponent.create();
        ClaseDependiente dep = component.getDependencia();
        dep.magiaDelCodigo();
    }
}
```

## Modulos

¿Qué ocurre si nuestra dependencia es una interfaz, Dagger fallará si nos ceñimos a lo que conocemos hasta ahora. Necesitamos darle más información, creando módulos.

Los módulos pueden definirse como Clases que pueden construir objetos de ciertos tipos.

Es importante saber que Dagger no modifica el funcionamiento del lenguaje, de las Clases. Si quieres integrar Dagger pero seguir el modo tradicional en algún momento o circunstancia, puedes hacerlo sin problemas.

Así que pasaremos de este código, que genera fallos:

```java
public class Dependencia1 implements InterfazInyectable {
    @Inject
    public Dependencia1() { }
}
```

... a:

```java
@Module
public class Dependencia1Module {
    @Provides
    InterfazInyectable provideInterfaz() {
        return new Dependencia1();
    }
}
```

Ahora, en nuestra Clase `AppComponent`, que se encarga de proveer nuestras dependencias, encontramos:

```java
@Component(modules = {Dependencia1Module.class})
interface AppComponent {
    ClaseDependiente getDependencia();
}
```
Los módulos reciben en sus métodos la anotación `@Provides`para indicar que pueden proveer de instancias de un determinado tipo. Recuerda que los nombres no son importantes para Dagger, solo las anotaciones en sus firmas.

## Referencias

Aprendiendo de Isoron en [A friendly introduction to Dagger 2][isoronintro]

---
#### [Volver al inicio][back]

[dagger]: https://google.github.io/dagger/
[addgradle]: https://gist.github.com/iSoron/3b52f16afdfd88687cf43f858786b0b6#file-build-gradle
[addmaven]: https://gist.github.com/iSoron/3b52f16afdfd88687cf43f858786b0b6#file-build-gradle

[isoronintro]: https://medium.com/@isoron/a-friendly-introduction-to-dagger-2-part-1-dbdf2f3fb17b

[back]: ../README.md