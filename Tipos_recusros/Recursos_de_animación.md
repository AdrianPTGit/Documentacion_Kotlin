# Recursos de animación

> Un recurso de animación puede definir uno de dos tipos de animaciones:

- **Animación de propiedades**
    - Crea una animación mediante la modificación de los valores de propiedades de un objeto durante un período establecido con un `Animator`.
- **Animación de vistas**

    - Hay dos tipos de animaciones que puedes hacer con el framework de animación de vistas:

        - **Animación de interpolación:** Crea una animación mediante una serie de transformaciones en una sola imagen con una `Animation`.
        - **Animación de marcos:** Crea una animación mediante una secuencia de imágenes en orden con un `AnimationDrawable`.


## Animación de propiedades

 > Se trata de una animación definida en XML que modifica las propiedades del objeto de destino, como el color de fondo o el valor Alfa, durante un período determinado.

#### ubicación del archivo:
- `res/animator/filename.xml`
- Se utiliza el nombre del archivo como ID de recurso.
#### tipo de datos de  compilados:
- Puntero del recurso en un `ValueAnimator`, `ObjectAnimator` o `AnimatorSet`
#### referencia del recurso:
- En código basado en Java o Kotlin: R.animator.filename
- En XML: `@[package:]animator/filename` 
#### sintaxis:
```xml
<set
  android:ordering=["together" | "sequentially"]>

    <objectAnimator
        android:propertyName="string"
        android:duration="int"
        android:valueFrom="float | int | color"
        android:valueTo="float | int | color"
        android:startOffset="int"
        android:repeatCount="int"
        android:repeatMode=["restart" | "reverse"]
        android:valueType=["intType" | "floatType"]/>

    <animator
        android:duration="int"
        android:valueFrom="float | int | color"
        android:valueTo="float | int | color"
        android:startOffset="int"
        android:repeatCount="int"
        android:repeatMode=["restart" | "reverse"]
        android:valueType=["intType" | "floatType"]/>

    <set>
        ...
    </set>
</set>
```

- El archivo debe tener un único elemento raíz: `<set>`, `<objectAnimator>` o `<valueAnimator>`. Puedes agrupar elementos de animación dentro del elemento `<set>`, incluidos otros elementos `<set>`. 

#### elementos:

- `<set>`
    - Un contenedor que contiene otros elementos de animación (`<objectAnimator>`, `<valueAnimator>` o algún otro elemento `<set>`). Representa un `AnimatorSet`.

    - Puedes especificar etiquetas de <set> anidadas para agrupar más animaciones. Cada <set> puede definir su propio atributo de ordering.

      - **Atributos**:

        - `android:ordering`
            - Palabra clave. Especifica el orden de reproducción de las animaciones en este conjunto. 
            - 
| Valor        | Descripción                                                  |
|---------------|--------------------------------------------------------------|
| `sequentially` | Reproduce animaciones en este conjunto de manera secuencial. |
| `together` *(predeterminado)* | Reproduce animaciones en este conjunto al mismo tiempo. |

`<objectAnimator>`
> Anima una propiedad específica de un objeto durante un lapso específico. Representa un `ObjectAnimator`.

- Atributos:

    - `android:propertyName`
        Cadena. Obligatoria. La propiedad del objeto que se animará, a la que se hace referencia por su nombre. 
        Por ejemplo, puedes especificar `alpha` o "`backgroundColor`" para un objeto `View`. Sin embargo, el elemento objectAnimator no expone un atributo target, por lo que no puedes establecer la animación del objeto en la declaración de XML. 
        Para aumentar el recurso XML de la animación, tienes que llamar a `loadAnimator()`; además, llama a `setTarget()` para establecer el objeto de destino que contiene esta propiedad. 
    - `android:valueTo`
        flotante, `int` o color. Obligatorio. Es el valor donde termina la propiedad animada. Los colores se representan como números hexadecimales de seis dígitos, como `#333333`. 
    - `android:valueFrom`
        flotante, `int` o `color`. Es el valor donde comienza la propiedad animada. 
        Si no se especifica, la animación comienza en el valor obtenido por el método get de la propiedad. 
        Los colores se representan como números hexadecimales de seis dígitos, como `#333333`. 
    - `android:duration`
        `int`. Es el tiempo en milisegundos de la animación. 300 milisegundos es el valor predeterminado. 
    - `android:startOffset`
        `int`. La cantidad de milisegundos que tarda la animación después de llamar a `start()`. 
    - `android:repeatCount`
        `int`. Cuántas veces repetir una animación. 
        Se establece en "`-1`" a fin de repetir infinitamente o para un número entero positivo. 
        Por ejemplo, un valor de "`1`" significa que la animación se repite una vez después de su ejecución inicial, de modo que se reproduce un total de dos veces.
        El valor predeterminado es "0", y esto significa que no hay repeticiones. 
    - `android:repeatMode`
        `int`. Cómo se comporta una animación cuando llega al final de la animación.
        `android:repeatCount` se debe establecer en un número entero positivo o "`-1`" para que este atributo tenga efecto. Configúralo en "**reverse**" de modo que la animación revierta la dirección con cada iteración o configúralo en "**restart**" para que se repita indefinidamente desde el principio cada vez. 
    - `android:valueType`
        Palabra clave. No especifiques este atributo si el valor es un color. El framework de animación maneja los valores de color automáticamente. 


`<animator>`
> Ejecuta una animación durante un lapso especificado. Representa un ValueAnimator.

- Atributos:

    - `android:valueTo`
        flotante, int o color. Obligatorio. Es el valor donde finaliza la animación. Los colores se representan como números hexadecimales de seis dígitos, como #333333. 
    - `android:valueFrom`
        flotante, int o color. Obligatorio. Es el valor donde comienza la animación. Los colores se representan como números hexadecimales de seis dígitos, como #333333. 
    - `android:duration`
        int. Es el tiempo en milisegundos de la animación. 300 milisegundos es el valor predeterminado. 
    - `android:startOffset`
        int. La cantidad de milisegundos que tarda la animación después de llamar a start(). 
    - `android:repeatCount`
        int. Cuántas veces repetir una animación. Se establece en "`-1`" a fin de repetir infinitamente o para un número entero positivo.
         Por ejemplo, un valor de "`1`" significa que la animación se repite una vez después de su ejecución inicial, de modo que se reproduce un total de dos veces. 
         El valor predeterminado es "`0`", y esto significa que no hay repeticiones. 
    - `android:repeatMode`
        int. Cómo se comporta una animación cuando llega al final de la animación. `android:repeatCount` se debe establecer en un número entero positivo o "-1" para que este atributo tenga efecto. Configúralo en "**reverse**" de modo que la animación revierta la dirección con cada iteración o configúralo en "**restart**" para que se repita indefinidamente desde el principio cada vez. 
    - `android:valueType`
        Palabra clave. No especifiques este atributo si el valor es un color.
         El framework de animación maneja los valores de color automáticamente. 

| Valor                     | Descripción                                                  |
|----------------------------|--------------------------------------------------------------|
| `intType`                  | Especifica que los valores animados son números enteros.     |
| `floatType` *(predeterminado)* | Especifica que los valores animados son números de punto flotante. |

- ejemplo:

    - Archivo en formato XML guardado en r`es/animator/property_animator.xml`:
```xml
<set android:ordering="sequentially">
    <set>
        <objectAnimator
            android:propertyName="x"
            android:duration="500"
            android:valueTo="400"
            android:valueType="intType"/>
        <objectAnimator
            android:propertyName="y"
            android:duration="500"
            android:valueTo="300"
            android:valueType="intType"/>
    </set>
    <objectAnimator
        android:propertyName="alpha"
        android:duration="500"
        android:valueTo="1f"/>
</set>
```

> Para ejecutar esta animación, aumenta los recursos XML en tu código a un objeto AnimatorSet y, luego, establece los objetos de destino para todas las animaciones antes de comenzar el conjunto de animación.
>  Llamar a `setTarget()` establece un único objeto de destino para todos los elementos secundarios del `AnimatorSet` como método de conveniencia.
> 
-  El siguiente código muestra cómo hacer esto:
  
```kotlin
val set: AnimatorSet = AnimatorInflater.loadAnimator(myContext, R.animator.property_animator)
    .apply {
        setTarget(myObject)
        start()
    }
```
## Animación de vistas

> El framework para la animación de vistas admite tanto animaciones de interpolación como por fotograma, y ambas se declaran en XML. En las secciones siguientes, se describe cómo usar ambos métodos.
- Animación de interpolación

  - Se trata de una animación definida en XML que realiza transiciones en un gráfico, como rotación, atenuación, movimiento y estiramiento.

- **Ubicación del archivo:**
  `res/anim/filename.xml`
    Se utiliza el nombre del archivo como ID de recurso.
- **tipo de datos de recursos compilados:**
    Puntero del recurso a un `Animation`
- **referencia del recurso:**
    En Java: `R.anim.filename`
    En XML: `@[package:]anim/filename` 