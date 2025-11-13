# Recurso de estilo

> Un **recurso de estilo** define el formato y el aspecto de una IU. Se puede aplicar un estilo a una `View` individual (desde dentro de un archivo de diseño), a una `Activity` completa o una aplicación (desde dentro del archivo de manifiesto).

Para obtener más información sobre cómo crear y aplicar estilos, lee Estilos y temas.

> **Nota:** Un estilo es un recurso simple al que se hace referencia mediante el valor proporcionado en el atributo name (no el nombre del archivo en formato XML). Como tal, puedes combinar recursos de estilo con otros recursos simples en el archivo en formato XML bajo un elemento <resources>.

## ubicación del archivo:
- `res/values/filename.xml`
- El nombre del archivo es arbitrario. Se usará el elemento name como el `ID` de recurso.
## referencia del recurso:
- En XML: `@[package:]style/style_name` 
## sintaxis:
```xml
    <?xml version="1.0" encoding="utf-8"?>
    <resources>
        <style
            name="style_name"
            parent="@[package:]style/style_to_inherit">
            <item
                name="[package:]style_property_name"
                >style_value</item>
        </style>
    </resources>
```
## elementos:

- `<resources>`
        Obligatorio. Este debe ser el nodo raíz.

        Sin atributos.
- `<style>`
        Define un estilo único. Contiene elementos <item>.

## atributos:

- `name`
    - String. Obligatoria. Es un nombre para el estilo, que se usa como `ID` de recurso a fin de aplicar ese estilo a un elemento `View`, `Activity` o aplicación. 
- `parent`
    - Recurso de estilo. Es una referencia a un estilo del que este estilo debe heredar las propiedades correspondientes. 

- `<item>`
    - Define una sola propiedad para el estilo. Debe ser un elemento secundario de `<style>`.

## atributos:

- `name`
    - **Recurso de atributo.** `Obligatori`o. El nombre de la propiedad de estilo que se definirá con un prefijo de paquete si es necesario (por ejemplo, `android:textColor`). 

## ejemplo:

- Archivo en formato XML para el estilo (guardado en `res/values/`):
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="CustomText" parent="@style/Text">
        <item name="android:textSize">20sp</item>
        <item name="android:textColor">#008</item>
    </style>
</resources>
```
- Archivo en formato XML que aplica el estilo a un TextView (guardado en `res/layout/`):
```xml
<?xml version="1.0" encoding="utf-8"?>
<EditText
    style="@style/CustomText"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
    android:text="Hello, World!" />
```

https://developer.android.com/guide/topics/resources/style-resource?hl=es-419