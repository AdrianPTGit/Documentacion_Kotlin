
# 🧠 Jetpack Compose: `remember` y `mutableStateOf`

## 1. Concepto general

Jetpack Compose utiliza un modelo **declarativo**: la UI se redibuja automáticamente cada vez que el estado cambia.
Para lograr esto, Compose necesita saber qué datos cambian y cuándo redibujar los Composables.

`remember` y `mutableStateOf` son los mecanismos que permiten manejar ese **estado reactivo**.

---

## ⚙️ 2. `mutableStateOf`: estado observable

```kotlin
val contador = mutableStateOf(0)
```

- Crea un **estado observable**.
- Cuando su valor cambia (`contador.value = nuevoValor`), Compose **redibuja automáticamente** los composables que lo usan.

### Propiedades principales
| Propiedad | Descripción |
|------------|--------------|
| `value` | Valor actual del estado. |
| `contador.value = x` | Asigna nuevo valor y dispara recomposición. |

### Ejemplo básico
```kotlin
val contador = mutableStateOf(0)

Button(onClick = { contador.value++ }) {
    Text("Clicks: ${contador.value}")
}
```

---

## 💾 3. `remember`: conserva valores entre recomposiciones

`remember` guarda valores **mientras el Composable siga activo**.
Sin él, las variables se reinician cada vez que Compose vuelve a ejecutar la función.

### Ejemplo incorrecto (sin `remember`)
```kotlin
@Composable
fun Contador() {
    var contador = mutableStateOf(0)
    Button(onClick = { contador.value++ }) {
        Text("Clicks: ${contador.value}")
    }
}
```
Cada vez que Compose redibuja, `contador` vuelve a 0.

### Ejemplo correcto
```kotlin
@Composable
fun Contador() {
    val contador = remember { mutableStateOf(0) }
    Button(onClick = { contador.value++ }) {
        Text("Clicks: ${contador.value}")
    }
}
```

---

## 🔄 4. Cómo funcionan juntos

| Elemento | Función |
|-----------|----------|
| `mutableStateOf` | Crea una variable **reactiva**. |
| `remember { ... }` | Conserva el valor entre recomposiciones. |

Ejemplo típico:
```kotlin
val nombre = remember { mutableStateOf("") }
```

---

## 🧩 5. Ejemplo completo

```kotlin
@Composable
fun SaludoInteractivo() {
    val nombre = remember { mutableStateOf("") }

    Column(modifier = Modifier.padding(16.dp)) {
        TextField(
            value = nombre.value,
            onValueChange = { nombre.value = it },
            label = { Text("Tu nombre") }
        )
        Spacer(modifier = Modifier.height(8.dp))
        Text(text = "Hola ${nombre.value}!")
    }
}
```

### Explicación
1. `mutableStateOf("")` crea el estado observable.  
2. `remember` lo conserva mientras el Composable viva.  
3. Compose redibuja automáticamente al cambiar `nombre.value`.

---

## 🧱 6. Uso con delegados (`by`)

Kotlin permite simplificar el código con el delegado `by`:

```kotlin
import androidx.compose.runtime.*

@Composable
fun Contador() {
    var contador by remember { mutableStateOf(0) }
    Button(onClick = { contador++ }) {
        Text("Clicks: $contador")
    }
}
```

✅ Es más limpio y se comporta igual.

---

## 🧰 7. Cuándo usar cada uno

| Situación | Qué usar | Descripción |
|------------|-----------|-------------|
| Estado simple (contador, texto, color) | `remember { mutableStateOf(...) }` | Se conserva entre recomposiciones. |
| Estado que debe sobrevivir a rotaciones | `rememberSaveable { mutableStateOf(...) }` | Se guarda en `SavedInstanceState`. |
| Estado compartido entre pantallas | `ViewModel` + `mutableStateOf` | Centraliza y persiste el estado. |

---

## ⚠️ 8. Qué ocurre si no usas `remember`

```kotlin
@Composable
fun SinRemember() {
    val contador = mutableStateOf(0)
    Button(onClick = { contador.value++ }) {
        Text("Contador: ${contador.value}")
    }
}
```
El valor de `contador` se reinicia en cada recomposición → el contador nunca pasa de 1.

---

## 🧩 9. Resumen visual

| Concepto | Qué hace | Se mantiene entre recomposiciones | Reactivo |
|-----------|-----------|----------------------------------|-----------|
| `mutableStateOf()` | Crea una variable observable | ❌ No | ✅ Sí |
| `remember { mutableStateOf() }` | Crea y guarda una variable observable | ✅ Sí | ✅ Sí |
| `rememberSaveable { mutableStateOf() }` | Igual que `remember`, pero sobrevive a rotaciones | ✅ (persistente) | ✅ Sí |

---

## 📚 10. Resumen final

- `mutableStateOf()` ➜ Estado observable.  
- `remember` ➜ Guarda el estado entre recomposiciones.  
- `rememberSaveable` ➜ Guarda el estado incluso tras cambios de configuración.  

Combinándolos correctamente, puedes construir **interfaces reactivas, estables y declarativas** en Compose.
