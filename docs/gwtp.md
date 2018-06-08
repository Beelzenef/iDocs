# GWTP

[GWTP][gwtp] es un _framework_ para el desarrollo de aplicaciones web en GWT, basado en MVP (ModeloVistaPresentador, _ModelViewPresenter_).

## Conceptos

* _Place_: La representación de una localización o _bookmark_ en la aplicación, a la que acceder mediante el botón de retroceso o marcadores del navegador. 
* _Proxy_: Clases ligeras que no dependen de la complejidad del paradigma Se instancian tan pronto como la aplicación carga, y son responsables de escuchar todos los eventos que requieran comunicación entre Vista y Presentador. Son la clave de la rapidez del MVP en aplicaciones web. No necesitas escribirlas, son creadas automáticamente por los generadores de GWT.
* _NameToken_: Es una anotación que permite marcar una página con una determinada URL.

Ref: [Creating places][places]

* _GIN_: En GWTP se usa la inyección de dependencias, como una base de su arquitectura. Permite crear aplicaciones con mejor mantenimiento. GIN es la librería que se usa para crear esa inyección de dependencias en GWTP. La inyección de dependencias[... es...][dependencyinjection]

Ref: [GIN Bindings][ginbindings]

## Introducción al _MVP_

El _Presenter_ o Presentador es el término en MVP que se refiere a la parte donde la lógica del cliente está escrita (validación, manipulación de la capa de modelo...) y la _View_ o Vista solo muestra los cambios marcados por el primero, sin ninguna lógica detrás.

El _Presenter_ se "reduce" a una Clase Java, mientras que la _View_ contiene una Clase Java y un fichero `ui.xml`, que es donde se definen los _widgets_, que serán accedidos desde Java.

### _Presenter slots_

GWTP introduce el concepto de [_Presenter slots_][presenterslots], que representan ranuras donde _Presenters_ hijos son agregados al padre, asociando esta a "familia" entre sí.

Hay varios tipos de _slots_:

* SingleSlot
* PermanentSlot
* Slot
* NestedSlot
* OrderedSlot
* PopupSlot: admite [_Popup presenters_][popuppresenters]

Las _slots_ pueden ser constantes, finales y estáticas, a los que añadiremos _presenters_ mediante `addToSlot()` o reemplazaremos un _presenter_ existente mediante `setInSlot()`.

Un presenter puede tener más de un _slot_.

## UiBinder

[UiBinder][uibinder] es el _framework_ con el que se especifican los widgets tanto de HTML como de GWT a través de XML.

```xml
<ui:UiBinder
xmlns:ui='urn:ui:com.google.gwt.uibinder'
xmlns:g='urn:import:com.google.gwt.user.client.ui'>

    <g:HTMLPanel>
        <p>hello there!</p>
    </g:HTMLPanel>
</ui:UiBinder>
```

Este fichero XML muestra un panel HTML con un mensaje en su interior. Podemos llamarlo `SaludoView.ui.xml`, y se asocia a la _View_ mediante:

```java
interface Binder extends UiBinder<Widget, SaludoView> { }
```

---
#### [Volver al inicio][back]

[gwtp]: http://dev.arcbees.com/gwtp/
[places]: http://dev.arcbees.com/gwtp/core/presenters/creating-places.html
[dependencyinjection]: https://dev.to/chrisvasqm/explain-dependency-injection-like-im-five
[ginbindings]: http://dev.arcbees.com/gwtp/core/presenters/gin-bindings.html
[presenterslots]: http://dev.arcbees.com/gwtp/core/slots/
[popuppresenters]: http://dev.arcbees.com/gwtp/core/presenters/popups.html
[uibinder]: http://www.gwtproject.org/doc/latest/DevGuideUiBinder.html

[back]: ../README.md