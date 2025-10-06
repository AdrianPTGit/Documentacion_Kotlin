
# 📘 Guía Técnica Jetpack Compose UI (`androidx.compose.ui`)

Esta guía contiene las **clases, propiedades y métodos más importantes** del paquete `androidx.compose.ui` y sus subpaquetes, con ejemplos prácticos.

---

## 🧩 1. Paquete principal

El paquete `androidx.compose.ui` contiene las bases del sistema de UI de Jetpack Compose: dibujo, medición, entrada táctil, texto y layout.

### Subpaquetes más relevantes
| Subpaquete | Descripción |
|-------------|-------------|
| `geometry` | Coordenadas y tamaños (`Offset`, `Size`, `Rect`). |
| `graphics` | Dibujo, colores, pinceles, paths (`Color`, `Canvas`, `Brush`). |
| `layout` | Medición y colocación de elementos (`Layout`, `MeasurePolicy`). |
| `text` | Estilos de texto (`TextStyle`, `FontFamily`). |
| `unit` | Unidades (`Dp`, `Sp`, `IntOffset`, `IntSize`). |
| `input` | Entrada táctil, teclado y foco (`pointer`, `key`, `focus`). |
| `platform` | Contexto Android, teclado, clipboard, densidad. |
| `tooling` | Soporte para `@Preview` en Android Studio. |

---

## ⚙️ 2. Clase `Modifier`

Permite encadenar transformaciones visuales o de comportamiento sobre un Composable.

### Propiedades principales
| Propiedad | Tipo | Descripción |
|------------|------|--------------|
| `Modifier.Companion` | `Modifier` | Representa un `Modifier` vacío. |
| `then(other: Modifier)` | `Modifier` | Combina dos modificadores. |

### Métodos comunes
| Método | Descripción |
|--------|--------------|
| `padding(all: Dp)` | Añade margen interno. |
| `background(color: Color)` | Fondo de color. |
| `border(width: Dp, color: Color)` | Borde exterior. |
| `fillMaxSize()` | Ocupa todo el espacio disponible. |
| `clickable(onClick: () -> Unit)` | Detecta toques. |
| `clip(shape: Shape)` | Recorta el contenido. |
| `offset(x: Dp, y: Dp)` | Desplaza el elemento. |
| `graphicsLayer { ... }` | Rotación, escala, opacidad. |
| `focusable()` | Recibe foco. |
| `zIndex(z: Float)` | Controla superposición. |

Ejemplo:
```kotlin
Modifier
    .padding(16.dp)
    .background(Color.Blue)
    .fillMaxWidth()
```

---

## 🎨 3. Clase `Color` (`androidx.compose.ui.graphics`)

Representa un color RGBA.

### Propiedades
| Propiedad | Tipo | Descripción |
|------------|------|--------------|
| `alpha` | `Float` | Transparencia (0–1). |
| `red` | `Float` | Canal rojo. |
| `green` | `Float` | Canal verde. |
| `blue` | `Float` | Canal azul. |
| `value` | `ULong` | Valor ARGB. |

### Métodos
| Método | Descripción |
|--------|--------------|
| `Color(red, green, blue, alpha)` | Crea color desde valores flotantes. |
| `Color(0xFF2196F3)` | Crea color desde valor hex. |
| `copy(alpha = …)` | Crea copia modificada. |

Ejemplo:
```kotlin
val azul = Color(0xFF2196F3)
val semi = azul.copy(alpha = 0.5f)
```

---

## 📝 4. Clase `TextStyle` (`androidx.compose.ui.text`)

Define los estilos de texto aplicados a `Text()`.

| Propiedad | Tipo | Descripción |
|------------|------|--------------|
| `color` | `Color` | Color del texto. |
| `fontSize` | `TextUnit` | Tamaño (`sp`). |
| `fontWeight` | `FontWeight` | Grosor. |
| `fontStyle` | `FontStyle` | Cursiva/normal. |
| `textAlign` | `TextAlign` | Alineación. |
| `textDecoration` | `TextDecoration` | Subrayado, tachado. |
| `background` | `Color` | Fondo. |
| `shadow` | `Shadow?` | Sombra. |

Ejemplo:
```kotlin
TextStyle(
    color = Color.Red,
    fontSize = 18.sp,
    fontWeight = FontWeight.Bold
)
```

---

## 📏 5. Clases `Dp` y `Sp` (`androidx.compose.ui.unit`)

### Dp (Density-independent pixels)
```kotlin
val padding = 16.dp
```

### Sp (Scale-independent pixels)
Usado para texto, respeta el tamaño del usuario.

Conversión:
```kotlin
with(LocalDensity.current) {
    val px = 20.dp.toPx()
}
```

---

## 📐 6. Clases geométricas (`androidx.compose.ui.geometry`)

| Clase | Propiedades | Descripción |
|--------|--------------|-------------|
| `Offset` | `x`, `y` | Coordenadas de un punto. |
| `Size` | `width`, `height` | Tamaño. |
| `Rect` | `left`, `top`, `right`, `bottom` | Rectángulo. |

Ejemplo:
```kotlin
val punto = Offset(10f, 20f)
val tamaño = Size(100f, 50f)
```

---

## 🖼️ 7. Dibujo (`Canvas`, `Path`)

### Canvas
| Método | Descripción |
|--------|--------------|
| `drawRect()` | Dibuja rectángulo. |
| `drawCircle()` | Dibuja círculo. |
| `drawPath()` | Dibuja ruta vectorial. |
| `drawLine()` | Dibuja línea. |

### Path
| Método | Descripción |
|--------|--------------|
| `moveTo(x, y)` | Mueve el cursor. |
| `lineTo(x, y)` | Dibuja línea. |
| `close()` | Cierra la figura. |

---

## 🖐️ 8. Entrada táctil (`PointerInputScope`)

| Método | Descripción |
|--------|--------------|
| `detectTapGestures(onTap)` | Detecta toques. |
| `detectDragGestures(onDrag)` | Detecta arrastres. |
| `detectTransformGestures(onGesture)` | Zoom/rotación. |

Ejemplo:
```kotlin
Modifier.pointerInput(Unit) {
    detectTapGestures {
        Log.d("Gesture", "Tocado")
    }
}
```

---

## 🧭 9. Plataforma (`LocalContext`)

Acceso al contexto Android:
```kotlin
val context = LocalContext.current
Toast.makeText(context, "Hola Compose", Toast.LENGTH_SHORT).show()
```

---

## 🧰 10. Herramientas de Previsualización (`@Preview`)

| Parámetro | Tipo | Descripción |
|------------|------|--------------|
| `showBackground` | `Boolean` | Fondo blanco. |
| `widthDp`, `heightDp` | `Int` | Tamaño. |
| `name` | `String` | Nombre del preview. |

Ejemplo:
```kotlin
@Preview(showBackground = true)
@Composable
fun CajaPreview() { CajaEjemplo() }
```

---

## 🧱 11. Clases adicionales

| Clase | Descripción |
|--------|--------------|
| `LayoutCoordinates` | Coordenadas del Composable. |
| `Density` | Conversión dp/sp ↔ px. |
| `IntOffset`, `IntSize` | Versiones enteras. |
| `Shadow` | Define sombra (`color`, `offset`, `blurRadius`). |

---

📚 **Fin de la guía Compose UI**
