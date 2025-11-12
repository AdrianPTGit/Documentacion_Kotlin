# 1. `Scaffold`
- Es un componente de diseño en Android mediante Jetpack Compose.
> El uso fundamental de este composable es tener una **estructura estándar** para los elementos visuales de la app, no es un elemento visual, más bien es un layout. Nos ayuda de manera muy **rápida** a **posicionar elementos** comunes en la **pantalla** sin programarlo de cero. También sigue el patrón de diseño Material Design.

## 1.1. Elementos de `scaffold`

- Los elementos que se pueden utilizar en un `scaffold` son los siguientes:

  - **Top app bar:** es la barra de la parte superior
  - **Content:** el contenido principal de la aplicación
  - **FAB (Floating Action Button):** botón flotante
  - **Bottom Bar:** es la barra de la parte inferior, o barra de navegación
  - **Drawer:** es el menú lateral que se expande/contrae

- Todos ellos son opcionales y personalizables, para hacer utilizar cada elemento, debemos generar un `Composable` para dicho elemento, como veremos en el uso básico.
## 1.2. Uso básico
### Función que define el `Scaffold` 
```kotlin
// Función Composable que crea un Scaffold personalizado
@Composable
fun CustomScaffold() {
    Scaffold(
        // Barra superior
        topBar = { CustomTopBar() },

        // Barra inferior
        bottomBar = { CustomBottomBar() },

        // Botón flotante personalizado
        floatingActionButton = { CustomFAB() }, 

        // Contenido principal
        content = { padding ->
            CustomContent(padding)
        }
    )
}

```

La función `Scaffold` tiene bastantes parámetros, yo en este caso he utilizado los siguientes:

- `topBar`: hace referencia a la barra superior, acepta un Composable de tipo TopAppBar`

- `bottomBar`: hace referencia a la barra inferior, por lo general su uso está destinado a la navegación dentro de la aplicación, acepta un Composable `BottomAppBar`

- `floatingActionButton`: es el botón flotante que se encuentra por encima de todos los demás elementos, se permite fusionar con la barra inferior, acepta un `FloatingActionButton`

- `content`: hace referencia al contenido principal de la aplicación, se puede poner cualquier Composable pero se suelen poner filas, columnas, surface, box, etc. En el ejemplo he utilizado Column

Estos cuatro parámetros aceptan funciones de tipo `@Composable` las que muestro a continuación:

### Barra superior
```kotlin
@Composable
fun CustomTopBar() {
    TopAppBar(
        // Título de la barra superior
        title = { Text(text = "Hello World!") }, 
    )
}
```
### Barra inferior
```kotlin
@Composable
fun CustomBottomBar() {
    BottomAppBar(content = {
        // Contenido de la barra inferior
        Text(text = "Item One")
    })
}
```

### Botón flotante

```kotlin
@Composable
fun CustomFAB() {
    FloatingActionButton(
        // Color de fondo
        backgroundColor = MaterialTheme.colors.primary,
        // Acción al hacer clic en el botón (sin definir)
        onClick = { /*TODO*/ }) { 
        Text(
            fontSize = 24.sp, // Tamaño de fuente del texto del botón
            text = "+" // Texto del botón
        )
    }
}
```

### Contenido principal

```kotlin
@Composable
fun CustomContent(padding: PaddingValues) {
    Column(
        // Modificadores de estilo de la columna
        modifier = Modifier
            // Ocupar todo el espacio disponible
            .fillMaxSize() 
            .padding(padding),

        // Contenido de la aplicación
        content = {
            Text(text = "My app content") 
        }
    )
}
```

