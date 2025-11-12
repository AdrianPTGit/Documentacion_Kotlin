# Recurso de lista de estados de color

> Un `ColorStateList` es un objeto que puedes definir en formato XML y aplicar como color, y que cambiará de color según el estado del objeto `View` al que se aplique.
>  Por ejemplo, un widget `Button` puede existir en uno de varios estados: presionado, enfocado o ninguno de estos. Con una lista de estados de color, puedes proporcionar un color diferente para cada estado.

Puedes describir la lista de estados en un archivo en formato XML. Cada color se define en un elemento `<item>` dentro de un único elemento `<selector>`. Cada `<item>` usa varios atributos para describir el estado en que se usa.

Durante cada cambio de estado, se recorre de arriba abajo la lista de estados, y se utiliza el primer elemento que coincida con el estado actual. La selección no depende de la "mejor coincidencia", sino del primer elemento que cumple con los criterios mínimos del estado.

- **ubicación del archivo:**
    res/color/filename.xml
    Se utiliza el nombre del archivo como ID de recurso.
- **tipo de datos de recursos compilados:**
    Puntero de recursos a un ColorStateList
- **referencia del recurso:**
    - En Java: `R.color.filename`
    - En XML: `@[package:]color/filename` 
- **sintaxis:**
```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android" >
    <item
        android:color="hex_color"
        android:lStar="floating_point_value"
        android:state_pressed=["true" | "false"]
        android:state_focused=["true" | "false"]
        android:state_selected=["true" | "false"]
        android:state_checkable=["true" | "false"]
        android:state_checked=["true" | "false"]
        android:state_enabled=["true" | "false"]
        android:state_window_focused=["true" | "false"] />
</selector>
```
- **elementos:**

    `<selector>`
        Obligatorio. Este es el elemento raíz. Contiene uno o más elementos `<item>`.

    - `Atributos:`

    - `xmlns:android`
            Cadena. Obligatoria. Define el espacio de nombres en formato XML, que es "http://schemas.android.com/apk/res/android". 

    <item>
        Define un color para usar durante ciertos estados, según la descripción de sus atributos. Es un elemento secundario de un elemento <selector>.

- **Atributos:**

    - android:color
        **Color hexadecimal.** Obligatorio. El color se especifica con un valor RGB y un canal Alfa opcional.

        El valor siempre comienza con un carácter numeral (`#`), seguido de la información `Alfa-Red-Green-Blue` (`Alfa-Rojo-Verde-Azul`) en uno de los siguientes formatos:

        - `#RGB`
        - `#ARVA`
        - `#RRVVAA`
        - `#AARRGGBB`

    - `android:lStar`
            Punto flotante. Opcional. Este atributo modifica la luminancia perceptual del color de base. Toma un valor de punto flotante entre `0` y `100` o un atributo de tema que se resuelve como tal.
            El color general del elemento se calcula convirtiendo el color de base en un espacio de color con mayor accesibilidad y configurando su `L*` en el valor que se especifica en el atributo lStar.

- **Ejemplo:** 
```xml 
android:lStar="50" 
```
    - `android:state_pressed`
            Booleano. Es "true" si se usa este elemento cuando se presiona el objeto, como cuando se toca un botón o se hace clic en él. Es "false" si se usa este elemento en el estado predeterminado, sin presionar.
    - `android:state_focused`
            Booleano. Es "`true`" si se usa este elemento cuando el objeto está enfocado, como cuando un botón se destaca con la bola de seguimiento o el pad direccional. 
            Es "`false`" si se usa este elemento en el estado predeterminado, no enfocado.
    - `android:state_selected`
            Booleano. Es "`true`" si se usa este elemento cuando el objeto está seleccionado, como cuando se abre una pestaña.
            Es "`false`" si se usa este elemento cuando el objeto no está seleccionado.
    - `android:state_checkable`
            Booleano. Es "`true`" si se usa este elemento cuando se puede marcar el objeto. 
            Es "fal`se" si se usa este elemento cuando no se puede marcar el objeto.
            Esto solo es útil si el objeto puede hacer la transición entre un widget que se puede marcar y uno que no.
    - `android:state_checked`
            Booleano. Es "true" si se usa este elemento cuando el objeto está marcado. Es "false" si se usa cuando el objeto no está seleccionado.
    - `android:state_enabled`
            Booleano. Es "`true`" si se usa este elemento cuando el objeto está habilitado y es capaz de recibir eventos táctiles o de clic.
            Es "`false`" si se usa cuando el objeto está `inhabilitado`.
    - `android:state_window_focuse`d
            Booleano. 
            Es "`true`" si se usa este elemento cuando la ventana de la aplicación está enfocada, lo que significa que la aplicación está en primer plano. 
            Es "false" si se usa este elemento cuando la ventana de la aplicación no está enfocada, por ejemplo, cuando se abre el panel de notificaciones o aparece un diálogo.

> **Nota:** Se aplica el primer elemento de la lista de estados que coincida con el estado actual del objeto. 
> Así, si el primer elemento de la lista no contiene ninguno de los atributos de estado anteriores, se aplicará cada vez.
>  Por este motivo, coloca tu valor predeterminado en último lugar, como se muestra en el siguiente ejemplo.

- **ejemplo:**
    Archivo en formato XML guardado en `res/color/button_text.xml`:
```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_pressed="true"
          android:color="#ffff0000"/> <!-- pressed -->
    <item android:state_focused="true"
          android:color="#ff0000ff"/> <!-- focused -->
    <item android:color="#ff000000"/> <!-- default -->
</selector>
```
En el siguiente XML de diseño, se aplica la lista de colores a un View:
```xml
    <Button
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="@string/button_text"
        android:textColor="@color/button_text" />
```
consulta también:

        Color (valor simple)
        ColorStateList
        Elemento de diseño de la lista de estados



