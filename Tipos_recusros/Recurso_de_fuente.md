# Recursos de fuente

> Un **recurso de fuente** define una fuente personalizada que puedes usar en tu app. Las fuentes pueden ser archivos individuales o una colección de varios, que se conoce como familia de fuentes y se define en XML.

También consulta cómo definir fuentes en XML o usar fuentes descargables.
## Paquete de fuentes

Puedes agrupar las fuentes como recursos de una app. Las fuentes se compilan en un archivo `R` y están disponibles automáticamente en el sistema como recurso. Luego, puedes acceder a ellas con la ayuda del tipo de recurso `font`.

ubicación del archivo:
    `res/font/filename.ttf` (`.ttf`, `.ttc`, `.otf` o `.xml`)
    Se usa el nombre del archivo como el ID de recurso. 
## referencia del recurso:
- En XML: `@[package:]font/font_name`
## sintaxis:
```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family>
  <font
    android:font="@[package:]font/font_to_include"
    android:fontStyle=["normal" | "italic"]
    android:fontWeight="weight_value" />
</font-family>
```

- elementos:

- `<font-family>`
        Obligatorio. Este debe ser el nodo raíz.

        Sin atributos.
- `<font>`
        Define una sola fuente dentro de una familia. No contiene nodos secundarios.

## Atributos:

- `android:fontStyle`
    - **Palabra clave.** Define el estilo de fuente. Cuando se carga la fuente en la pila de fuentes, se usa este atributo, que anula cualquier información de estilo en las tablas de encabezado de la fuente.
    Si no especificas el atributo, la app usa el valor de las tablas de encabezado de la fuente. El valor constante es `normal` o `italic`. 
- `android:fontWeight`
    - **Entero.** El grosor de la fuente. Cuando se carga la fuente en la pila de fuentes, se usa este atributo, que anula cualquier información de grosor incluida en las tablas del encabezado de la fuente.
    - El valor del atributo debe ser un múltiplo de `100` entre `100` y `900`, inclusive.
    - Si no especificas el atributo, la app usa el valor de las tablas de encabezado de la fuente. Los valores más comunes son `400` para el grosor normal y `700` para la negrita. 

## ejemplo:
- Archivo en formato XML guardado en `res/font/lobster.xml`:
```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android">
    <font
        android:fontStyle="normal"
        android:fontWeight="400"
        android:font="@font/lobster_regular" />
    <font
        android:fontStyle="italic"
        android:fontWeight="400"
        android:font="@font/lobster_italic" />
</font-family>
```

# Fuente descargable

Un recurso de fuente descargable define una fuente personalizada que puedes usar en una app. Esta fuente no está disponible en la app. En cambio, la fuente se recupera de un proveedor de fuentes.

## ubicación del archivo:
    res/font/filename.xml El nombre del archivo es el ID del recurso. 
## referencia del recurso:
- En XML: `@[package:]font/font_name` 
## sintaxis:
```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family
    android:fontProviderAuthority="authority"
    android:fontProviderPackage="package"
    android:fontProviderQuery="query"
    android:fontProviderCerts="@[package:]array/array_resource" />
```
## elementos:

<font-family>

> Obligatorio. Este debe ser el nodo raíz.

## atributos:

- `android:fontProviderAuthority`
    - `String`. **Obligatoria**. Es la autoridad del proveedor de fuentes que define la solicitud. 
- `android:fontProviderPackage`
    - `String`. **Obligatoria**. Es el nombre del paquete del proveedor de fuentes que se utilizará para la solicitud. Se usa para verificar la identidad del proveedor. 
- `android:fontProviderQuery`
    - `String`. **Obligatoria**. Es la búsqueda de string de la fuente. Consulta la documentación de tu proveedor de fuentes para obtener más información sobre el formato de esta string. 
- `android:fontProviderCerts`
    - Recurso de array. **Obligatorio**. Define los conjuntos de hashes de los certificados que se usan para firmar este proveedor. 
    - Se usa para verificar la identidad del proveedor y solo es necesario si ese proveedor no es parte de la imagen del sistema.
    - El valor puede apuntar a una sola lista (recurso de array de cadenas) o una lista de listas (recurso de array), donde cada lista individual representa una colección de hashes de firma. 
    - Consulta la documentación de tu proveedor de fuentes para obtener estos valores. 

## ejemplo:
- Archivo en formato XML guardado en `res/font/lobster.xml`:
```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
    android:fontProviderAuthority="com.example.fontprovider.authority"
    android:fontProviderPackage="com.example.fontprovider"
    android:fontProviderQuery="Lobster"
    android:fontProviderCerts="@array/certs">
</font-family>
```

- Archivo en formato XML guardado en `res/values/` que define el array de `cert`:
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="certs">
      <item>MIIEqDCCA5CgAwIBAgIJA071MA0GCSqGSIb3DQEBBAUAMIGUMQsww...</item>
    </string-array>
</resources>
```
- Archivo en formato XML guardado en `res/layout/` que aplica la fuente a un elemento `TextView`:
```xml
<?xml version="1.0" encoding="utf-8"?>
<EditText
    android:fontFamily="@font/lobster"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="Hello, World!" />
```

https://developer.android.com/guide/topics/resources/font-resource?hl=es-419