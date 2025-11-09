# ¿Qué es **`Responsive Views Activity`** en Android Studio

> La plantilla **`Responsive Views Activity`** es una actividad preconfigurada que Google añadió para ayudarte a crear una app que se adapte automáticamente a móviles, tablets y pantallas grandes.

 - Está pensada especialmente para usar con Jetpack Compose, aunque internamente también puede mezclar código XML y Compose.

##  Objetivo principal

- El **objetivo de esta plantilla** es que no tengas que diseñar dos layouts distintos (uno para móvil y otro para tablet).
- En su lugar, la plantilla usa **layouts adaptativos** que reorganizan los elementos según el ancho de pantalla disponible.

## Qué incluye la plantilla

- Cuando eliges Responsive Views Activity al crear tu proyecto, Android Studio genera varios archivos listos para usar:

| Archivo / Carpeta          | Descripción                                                                 |
|-----------------------------|------------------------------------------------------------------------------|
| **`MainActivity.kt`**         | La Activity principal que controla la UI responsive.                        |
| **`ui/theme/`**               | Archivos de tema (colores, tipografía, formas).                             |
| **`navigation/`**             | Control de navegación adaptable.                                            |
| **`ui/adaptive/`**            | Composables que organizan la vista según el tamaño de pantalla (Compact, Medium, Expanded). |
| **`ResponsiveContent.kt`**    | Define cómo se muestran las vistas según el tamaño de pantalla.             |

## Cómo funciona

- El núcleo del sistema responsive usa la clase:
```kotlin
WindowAdaptiveInfo
```
- que detecta el tamaño de la ventana actual.

    - Luego, según el ancho de pantalla, la app decide qué tipo de layout usar:

| Tipo de pantalla | Clasificación              | Ejemplo de layout                                 |
|------------------|----------------------------|---------------------------------------------------|
| **Compact**      | Móviles pequeños           | Vista en columna (una sola pantalla)              |
| **Medium**       | Tablets pequeñas           | Vista dividida (dos paneles verticales)           |
| **Expanded**     | Tablets grandes / escritorio | Vista con tres paneles o layout en fila          |

### Ejemplo dentro del código generado

```kotlin
@Composable
fun MainResponsiveScreen(windowSizeClass: WindowSizeClass) {
    when (windowSizeClass.widthSizeClass) {
        WindowWidthSizeClass.Compact -> {
            CompactView()
        }
        WindowWidthSizeClass.Medium -> {
            MediumView()
        }
        WindowWidthSizeClass.Expanded -> {
            ExpandedView()
        }
    }
}
```

- `CompactView()` → diseño para móvil
- `MediumView()` → diseño tipo panel dividido
- `ExpandedView()` → diseño amplio para pantallas grandes

 > 🧰 Ventajas
✅ Diseño automático adaptable a cualquier tamaño.
✅ Código base moderno con Material 3 y Jetpack Compose.
✅ Ideal para apps multiplataforma (Android, tablets, Chromebooks).
✅ Ya incluye navegación adaptativa.

| 🚀 En resumen | Descripción |
|-----------------------------|-------------------------------------------------------------|
| **Responsive Views Activity** | Plantilla moderna que crea una interfaz adaptable. |
| **Usa Jetpack Compose** | Se apoya en Material 3 y WindowSizeClass. |
| **Genera 3 vistas** | Compact (móvil), Medium (tablet), Expanded (pantalla grande). |
| **Objetivo** | Que tu app se vea bien sin crear layouts separados. |

