# Crear `appBar`

> Como crear un `appBar` que se muestre en todas las pantallas de la App tenga color rojo de fondo y texto en  color blanco.

# Paso 1: Crear un Composable reutilizable para la AppBar

Crea un archivo llamado `AppBarPersonalizada.kt` (por ejemplo en `ui/componentes/`):

```kotlin
package com.example.tuapp.ui.componentes

import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.graphics.Color

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun AppBarPersonalizada(titulo: String) {
    CenterAlignedTopAppBar(
        title = {
            Text(
                text = titulo,
                color = Color.White // Texto blanco
            )
        },
        colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
            containerColor = Color(0xFFD32F2F), // Fondo rojo (puedes cambiarlo)
            titleContentColor = Color.White
        )
    )
}
```
# Paso 2: Usar la AppBar en cada pantalla

- Por ejemplo, si tienes una pantalla llamada `PantallaInicio.kt`:

```kotlin
package com.example.tuapp.ui.pantallas

import androidx.compose.foundation.layout.*
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import com.example.tuapp.ui.componentes.AppBarPersonalizada

@Composable
fun PantallaInicio() {
    Scaffold(
        topBar = { AppBarPersonalizada("Inicio") }
    ) { paddingValues ->
        // Contenido de la pantalla
        Column(
            modifier = Modifier
                .padding(paddingValues)
                .fillMaxSize(),
            verticalArrangement = Arrangement.Center,
            horizontalAlignment = androidx.compose.ui.Alignment.CenterHorizontally
        ) {
            Text(text = "Bienvenido a la pantalla de inicio")
        }
    }
}

```

- Y si creas otra, por ejemplo `PantallaPerfil.kt`:

```kotlin
@Composable
fun PantallaPerfil() {
    Scaffold(
        topBar = { AppBarPersonalizada("Perfil") }
    ) { paddingValues ->
        Box(
            modifier = Modifier
                .padding(paddingValues)
                .fillMaxSize(),
            contentAlignment = androidx.compose.ui.Alignment.Center
        ) {
            Text(text = "Esta es la pantalla de Perfil")
        }
    }
}

```

# Resultado

- Cada pantalla:

    - Tendrá su AppBar roja (#D32F2F).
    - El texto del título será blanco.
    - Podrás cambiar el texto del título según la pantalla.


#  AppBar con botón de retroceso
## Paso 1: 

- Crea (o edita) tu archivo `AppBarPersonalizada.kt`:

```kotlin
package com.example.tuapp.ui.componentes

import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.ArrowBack
import androidx.compose.material3.*
import androidx.compose.runtime.Composable
import androidx.compose.ui.graphics.Color
import androidx.navigation.NavController

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun AppBarPersonalizada(
    titulo: String,
    navController: NavController? = null, // opcional
    mostrarFlecha: Boolean = false
) {
    CenterAlignedTopAppBar(
        title = {
            Text(
                text = titulo,
                color = Color.White
            )
        },
        navigationIcon = {
            if (mostrarFlecha) {
                IconButton(onClick = { navController?.popBackStack() }) {
                    Icon(
                        imageVector = Icons.Default.ArrowBack,
                        contentDescription = "Volver",
                        tint = Color.White
                    )
                }
            }
        },
        colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
            containerColor = Color(0xFFD32F2F), // rojo
            titleContentColor = Color.White
        )
    )
}
```

## Paso 2: Usar la AppBar en cada pantalla

### Pantalla principal **(sin flecha):**

```kotlin

@Composable
fun PantallaInicio(navController: NavController) {
    Scaffold(
        topBar = { AppBarPersonalizada("Inicio") }
    ) { paddingValues ->
        Column(
            modifier = Modifier
                .padding(paddingValues)
                .fillMaxSize(),
            verticalArrangement = Arrangement.Center,
            horizontalAlignment = androidx.compose.ui.Alignment.CenterHorizontally
        ) {
            Text("Pantalla de inicio")
            Button(onClick = { navController.navigate("perfil") }) {
                Text("Ir al perfil")
            }
        }
    }
}
```
### Pantalla secundaria **(con flecha de volver)**

```kotlin
@Composable
fun PantallaPerfil(navController: NavController) {
    Scaffold(
        topBar = {
            AppBarPersonalizada(
                titulo = "Perfil",
                navController = navController,
                mostrarFlecha = true
            )
        }
    ) { paddingValues ->
        Box(
            modifier = Modifier
                .padding(paddingValues)
                .fillMaxSize(),
            contentAlignment = androidx.compose.ui.Alignment.Center
        ) {
            Text("Pantalla de perfil")
        }
    }
}
```

## Paso 3: Configurar navegación (si aún no lo tienes)

- En tu archivo de navegación principal, por ejemplo `NavGraph.kt`:

```kotlin
import androidx.navigation.NavHostController
import androidx.navigation.compose.NavHost
import androidx.navigation.compose.composable

@Composable
fun NavGraph(navController: NavHostController) {
    NavHost(navController, startDestination = "inicio") {
        composable("inicio") { PantallaInicio(navController) }
        composable("perfil") { PantallaPerfil(navController) }
    }
}
```

Y en tu `MainActivity`:
```kotlin
setContent {
    val navController = rememberNavController()
    NavGraph(navController)
}
```
> 🎯 **Resultado final**
✅ Cada pantalla tiene su AppBar roja con texto blanco.
✅ Las pantallas secundarias muestran una flecha de retroceso funcional.
✅ Todo es reutilizable y limpio.