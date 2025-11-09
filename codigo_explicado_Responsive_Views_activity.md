# `MainActivity.kt`

## 🧩 En resumen

| Elemento | Función |
|-----------|----------|
| **`AppBarConfiguration`** | Gestiona la relación entre la AppBar y la navegación (`Drawer` / `BottomNav`). |
| **`NavigationView` / `BottomNavView`** | Permiten cambiar entre pantallas con el mismo NavController. |
| **`Snackbar` + `FAB`** | Muestra una acción flotante temporal. |
| **`ViewBinding`** | Sustituye a `findViewById`, accediendo a las vistas de forma segura. |
| **`onSupportNavigateUp()`** | Habilita la navegación hacia atrás desde la barra superior. |



```kotlin
// Paquete principal del proyecto.
package com.example.appresponsive

// Importaciones necesarias para componentes de Android y Jetpack Navigation.
import android.os.Bundle
import android.view.Menu
import android.view.MenuItem
import com.google.android.material.snackbar.Snackbar
import com.google.android.material.navigation.NavigationView
import androidx.navigation.findNavController
import androidx.navigation.fragment.NavHostFragment
import androidx.navigation.ui.AppBarConfiguration
import androidx.navigation.ui.navigateUp
import androidx.navigation.ui.setupActionBarWithNavController
import androidx.navigation.ui.setupWithNavController
import androidx.appcompat.app.AppCompatActivity
import com.example.appresponsive.databinding.ActivityMainBinding

// ------------------------------------------------------------
// Clase principal de la aplicación.
// Controla el contenido principal, la barra de navegación y la UI adaptable.
// ------------------------------------------------------------
class MainActivity : AppCompatActivity() {

    // Configuración del AppBar (para manejar el botón "Up" y el Drawer)
    private lateinit var appBarConfiguration: AppBarConfiguration

    // Binding generado automáticamente para acceder a las vistas del layout activity_main.xml
    private lateinit var binding: ActivityMainBinding

    // ------------------------------------------------------------
    // Método principal que se ejecuta al iniciar la Activity
    // ------------------------------------------------------------
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        // Se infla el layout usando ViewBinding en lugar de setContentView tradicional.
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        // Configura la Toolbar como la barra de acción principal.
        setSupportActionBar(binding.appBarMain.toolbar)

        // Configura la acción del botón flotante (FAB)
        // Muestra un Snackbar con un mensaje al hacer clic.
        binding.appBarMain.fab?.setOnClickListener { view ->
            Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
                .setAction("Action", null)
                .setAnchorView(R.id.fab) // Ancla el Snackbar al FAB
                .show()
        }

        // ------------------------------------------------------------
        // Configuración del controlador de navegación (NavController)
        // ------------------------------------------------------------

        // Se obtiene el NavHostFragment del layout principal.
        // El NavHost contiene los fragmentos (pantallas) gestionados por Navigation Component.
        val navHostFragment =
            (supportFragmentManager.findFragmentById(R.id.nav_host_fragment_content_main) as NavHostFragment?)!!
        val navController = navHostFragment.navController

        // ------------------------------------------------------------
        // Configuración del Navigation Drawer (menú lateral)
        // Solo se aplica si existe en el layout actual.
        // ------------------------------------------------------------
        binding.navView?.let {
            appBarConfiguration = AppBarConfiguration(
                setOf(
                    // Fragmentos principales accesibles desde el Drawer
                    R.id.nav_transform, 
                    R.id.nav_reflow, 
                    R.id.nav_slideshow, 
                    R.id.nav_settings
                ),
                binding.drawerLayout // Vincula el Drawer con el NavController
            )

            // Sincroniza la ActionBar con el NavController y el Drawer.
            setupActionBarWithNavController(navController, appBarConfiguration)

            // Conecta el NavigationView (menú lateral) con el NavController.
            it.setupWithNavController(navController)
        }

        // ------------------------------------------------------------
        // Configuración del Bottom Navigation (menú inferior)
        // Solo se aplica si el layout incluye bottomNavView.
        // ------------------------------------------------------------
        binding.appBarMain.contentMain.bottomNavView?.let {
            appBarConfiguration = AppBarConfiguration(
                setOf(
                    // Fragmentos principales accesibles desde el menú inferior
                    R.id.nav_transform,
                    R.id.nav_reflow,
                    R.id.nav_slideshow
                )
            )

            // Configura la ActionBar para que muestre el botón “Up” si es necesario.
            setupActionBarWithNavController(navController, appBarConfiguration)

            // Vincula el menú inferior con el NavController.
            it.setupWithNavController(navController)
        }
    }

    // ------------------------------------------------------------
    // Crea el menú de opciones (los tres puntos en la AppBar)
    // ------------------------------------------------------------
    override fun onCreateOptionsMenu(menu: Menu): Boolean {
        val result = super.onCreateOptionsMenu(menu)

        // Usamos findViewById porque NavigationView puede no existir en todos los layouts
        // (por ejemplo, cambia entre móvil y tablet).
        val navView: NavigationView? = findViewById(R.id.nav_view)

        // Si el menú lateral no está visible, inflamos un menú de desbordamiento (overflow)
        if (navView == null) {
            // Crea un menú adicional (por ejemplo, "Ajustes") cuando no hay Drawer.
            menuInflater.inflate(R.menu.overflow, menu)
        }

        return result
    }

    // ------------------------------------------------------------
    // Maneja los clics en los ítems del menú (AppBar)
    // ------------------------------------------------------------
    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when (item.itemId) {
            // Si el usuario selecciona "Configuración", navega a esa pantalla
            R.id.nav_settings -> {
                val navController = findNavController(R.id.nav_host_fragment_content_main)
                navController.navigate(R.id.nav_settings)
            }
        }
        return super.onOptionsItemSelected(item)
    }

    // ------------------------------------------------------------
    // Permite la navegación "Up" (flecha atrás en la AppBar)
    // ------------------------------------------------------------
    override fun onSupportNavigateUp(): Boolean {
        val navController = findNavController(R.id.nav_host_fragment_content_main)
        return navController.navigateUp(appBarConfiguration) || super.onSupportNavigateUp()
    }
}
```
# `reflow/ReflowFragment.kt`

## En resumen

| Elemento | Función |
|-----------|----------|
| **`ReflowFragment`** | Muestra la interfaz de la sección “Reflow”. |
| **`ViewBinding (FragmentReflowBinding)`** | Permite acceder a las vistas del XML sin usar `findViewById()`. |
| **`ViewModel (ReflowViewModel)`** | Mantiene los datos visibles incluso al rotar la pantalla. |
| **`LiveData.observe()`** | Detecta cambios en los datos y actualiza automáticamente la UI. |
| **`onDestroyView()`** | Libera el binding para evitar fugas de memoria. |

```kotlin
// Paquete donde se encuentra este fragmento.
// Agrupa la lógica y vistas relacionadas con la sección "Reflow" de la app.
package com.example.appresponsive.ui.reflow

// Importaciones necesarias para usar Fragmentos, Vistas y ViewBinding.
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.fragment.app.Fragment
import androidx.lifecycle.ViewModelProvider
import com.example.appresponsive.databinding.FragmentReflowBinding

// ------------------------------------------------------------
// Clase ReflowFragment
// Representa una pantalla (fragmento) dentro de la aplicación.
// Está asociada a un layout XML (fragment_reflow.xml) y a un ViewModel.
// ------------------------------------------------------------
class ReflowFragment : Fragment() {

    // Variable privada para el objeto de binding del layout.
    // El prefijo "_" indica que es una referencia temporal (puede ser null).
    private var _binding: FragmentReflowBinding? = null

    // Propiedad pública para acceder al binding de forma segura.
    // Solo se puede usar entre onCreateView() y onDestroyView().
    private val binding get() = _binding!!

    // ------------------------------------------------------------
    // Método que crea y configura la vista del fragmento.
    // Se ejecuta cuando el fragmento debe mostrar su interfaz de usuario.
    // ------------------------------------------------------------
    override fun onCreateView(
        inflater: LayoutInflater,       // Permite inflar (crear) vistas desde XML.
        container: ViewGroup?,          // Contenedor padre del fragmento.
        savedInstanceState: Bundle?     // Datos guardados en caso de recreación.
    ): View {

        // Crea (o recupera) el ViewModel asociado a este fragmento.
        // El ViewModel mantiene los datos al rotar la pantalla o recrear la vista.
        val reflowViewModel =
            ViewModelProvider(this).get(ReflowViewModel::class.java)

        // Infla el layout XML y lo vincula a través de ViewBinding.
        // Esto reemplaza a "findViewById" con acceso seguro a las vistas.
        _binding = FragmentReflowBinding.inflate(inflater, container, false)

        // "root" es la vista raíz del layout inflado (el contenedor principal).
        val root: View = binding.root

        // ------------------------------------------------------------
        // Conexión entre el ViewModel y el TextView de la interfaz.
        // ------------------------------------------------------------

        // Obtiene la referencia al TextView definido en el layout (textReflow).
        val textView: TextView = binding.textReflow

        // Observa el valor del LiveData "text" del ViewModel.
        // Cada vez que cambie el valor en el ViewModel, se actualiza el texto en pantalla.
        reflowViewModel.text.observe(viewLifecycleOwner) {
            textView.text = it
        }

        // Devuelve la vista raíz para que el sistema la muestre en pantalla.
        return root
    }

    // ------------------------------------------------------------
    // Método llamado cuando la vista del fragmento se destruye.
    // Se limpia la referencia del binding para evitar fugas de memoria (memory leaks).
    // ------------------------------------------------------------
    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

# `reflow/ReflowViewModel.kt`
## En resumen

| Elemento | Función |
|-----------|----------|
| **`ViewModel`** | Mantiene los datos de la UI de forma persistente, incluso al rotar la pantalla. |
| **`MutableLiveData`** | Permite almacenar y modificar datos que la UI puede observar. |
| **`LiveData (público)`** | Permite que la UI observe los datos sin poder modificarlos directamente. |
| **`apply { value = ... }`** | Inicializa el valor de la variable al crear el ViewModel. |

```kotlin
// Paquete donde se encuentra el ViewModel
// Relacionado con la sección "Reflow" de la app
package com.example.appresponsive.ui.reflow

// Importaciones necesarias para usar ViewModel y LiveData
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel

// ------------------------------------------------------------
// Clase ReflowViewModel
// Mantiene los datos que la UI del fragmento ReflowFragment mostrará.
// ------------------------------------------------------------
class ReflowViewModel : ViewModel() {

    // Variable privada de tipo MutableLiveData
    // MutableLiveData permite modificar el valor internamente.
    private val _text = MutableLiveData<String>().apply {
        // Valor inicial que se mostrará en la UI al crear el fragmento
        value = "This is reflow Fragment"
    }

    // Exposición del LiveData público (solo lectura) para la UI
    // Esto permite que otros componentes (como el Fragment) observen los cambios
    // sin poder modificar el valor directamente.
    val text: LiveData<String> = _text
}
```
# `settings/SettingsFragment.kt`

## En resumen

| Elemento | Función |
|-----------|----------|
| **`SettingsFragment`** | Pantalla de configuración de la app. |
| **`ViewBinding (FragmentSettingsBinding)`** | Permite acceder a las vistas del XML sin `findViewById()`. |
| **V`iewModel (SettingsViewModel)`** | Mantiene los datos visibles incluso al rotar la pantalla. |
| **`LiveData.observe()`** | Actualiza automáticamente la UI cuando los datos cambian. |
| **`onDestroyView()`** | Libera la referencia del binding para evitar memory leaks. |



```kotlin
// Paquete donde se encuentra el fragmento
// Agrupa la lógica y vistas relacionadas con la sección "Settings" de la app
package com.example.appresponsive.ui.settings

// Importaciones necesarias para Fragment, vistas y ViewModel
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.fragment.app.Fragment
import androidx.lifecycle.ViewModelProvider
import com.example.appresponsive.databinding.FragmentSettingsBinding

// ------------------------------------------------------------
// Clase SettingsFragment
// Representa la pantalla de configuración de la aplicación.
// ------------------------------------------------------------
class SettingsFragment : Fragment() {

    // Variable privada para el binding del layout fragment_settings.xml
    // Puede ser null cuando la vista no existe
    private var _binding: FragmentSettingsBinding? = null

    // Propiedad pública para acceder al binding de forma segura
    // Solo válida entre onCreateView() y onDestroyView()
    private val binding get() = _binding!!

    // ------------------------------------------------------------
    // Método que crea y configura la vista del fragmento
    // ------------------------------------------------------------
    override fun onCreateView(
        inflater: LayoutInflater,       // Infla vistas desde XML
        container: ViewGroup?,          // Contenedor padre del fragmento
        savedInstanceState: Bundle?     // Datos guardados en caso de recreación
    ): View {

        // Crea (o obtiene) el ViewModel asociado a este fragmento
        // Mantiene los datos de la UI incluso si se rota la pantalla
        val settingsViewModel =
            ViewModelProvider(this).get(SettingsViewModel::class.java)

        // Infla el layout usando ViewBinding
        _binding = FragmentSettingsBinding.inflate(inflater, container, false)

        // Guarda la vista raíz para devolverla al sistema
        val root: View = binding.root

        // ------------------------------------------------------------
        // Conexión entre ViewModel y TextView del layout
        // ------------------------------------------------------------

        // Referencia al TextView definido en fragment_settings.xml
        val textView: TextView = binding.textSettings

        // Observa los cambios en el LiveData del ViewModel
        // Cada vez que cambie el valor, se actualiza automáticamente el TextView
        settingsViewModel.text.observe(viewLifecycleOwner) {
            textView.text = it
        }

        // Devuelve la vista raíz para que se muestre en pantalla
        return root
    }

    // ------------------------------------------------------------
    // Método llamado cuando la vista del fragmento se destruye
    // Se limpia la referencia del binding para evitar fugas de memoria
    // ------------------------------------------------------------
    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```
# `settings/SettingsViewModel.kt`
```kotlin

// Paquete donde se encuentra el ViewModel
// Relacionado con la sección "Settings" de la app
package com.example.appresponsive.ui.settings

// Importaciones necesarias para ViewModel y LiveData
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel

// ------------------------------------------------------------
// Clase SettingsViewModel
// Mantiene los datos que la UI del SettingsFragment mostrará.
// ------------------------------------------------------------
class SettingsViewModel : ViewModel() {

    // Variable privada MutableLiveData
    // Permite modificar internamente el valor que observa la UI
    private val _text = MutableLiveData<String>().apply {
        // Valor inicial que se mostrará en la pantalla de Settings
        value = "This is settings Fragment"
    }

    // Exposición del LiveData público (solo lectura) para la UI
    // Permite que SettingsFragment observe los cambios sin poder modificar el valor directamente
    val text: LiveData<String> = _text
}

```

En resumen

| Elemento | Función |
|-----------|----------|
| **`SettingsFragment`** | Pantalla de configuración de la app. |
| **`ViewBinding (FragmentSettingsBinding)`** | Permite acceder a las vistas del XML sin `findViewById()`. |
| **`ViewModel (SettingsViewModel)`** | Mantiene los datos visibles incluso al rotar la pantalla. |
| **`LiveData.observe()`** | Actualiza automáticamente la UI cuando los datos cambian. |
| **`onDestroyView()`** | Libera la referencia del binding para evitar memory leaks. |

# `Slideshow/SlideshowFragment.kt`

```kotlin
// Paquete donde se encuentra el fragmento
// Agrupa la lógica y vistas relacionadas con la sección "Slideshow" de la app
package com.example.appresponsive.ui.slideshow

// Importaciones necesarias para Fragment, vistas y ViewModel
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.fragment.app.Fragment
import androidx.lifecycle.ViewModelProvider
import com.example.appresponsive.databinding.FragmentSlideshowBinding

// ------------------------------------------------------------
// Clase SlideshowFragment
// Representa la pantalla de presentación o slideshow de la app
// ------------------------------------------------------------
class SlideshowFragment : Fragment() {

    // Variable privada para el binding del layout fragment_slideshow.xml
    // Puede ser null cuando la vista no existe
    private var _binding: FragmentSlideshowBinding? = null

    // Propiedad pública para acceder al binding de forma segura
    // Solo válida entre onCreateView() y onDestroyView()
    private val binding get() = _binding!!

    // ------------------------------------------------------------
    // Método que crea y configura la vista del fragmento
    // ------------------------------------------------------------
    override fun onCreateView(
        inflater: LayoutInflater,       // Infla vistas desde XML
        container: ViewGroup?,          // Contenedor padre del fragmento
        savedInstanceState: Bundle?     // Datos guardados en caso de recreación
    ): View {

        // Crea (o obtiene) el ViewModel asociado a este fragmento
        // Mantiene los datos de la UI incluso si se rota la pantalla
        val slideshowViewModel =
            ViewModelProvider(this).get(SlideshowViewModel::class.java)

        // Infla el layout usando ViewBinding
        _binding = FragmentSlideshowBinding.inflate(inflater, container, false)

        // Guarda la vista raíz para devolverla al sistema
        val root: View = binding.root

        // ------------------------------------------------------------
        // Conexión entre ViewModel y TextView del layout
        // ------------------------------------------------------------

        // Referencia al TextView definido en fragment_slideshow.xml
        val textView: TextView = binding.textSlideshow

        // Observa los cambios en el LiveData del ViewModel
        // Cada vez que cambie el valor, se actualiza automáticamente el TextView
        slideshowViewModel.text.observe(viewLifecycleOwner) {
            textView.text = it
        }

        // Devuelve la vista raíz para que se muestre en pantalla
        return root
    }

    // ------------------------------------------------------------
    // Método llamado cuando la vista del fragmento se destruye
    // Se limpia la referencia del binding para evitar fugas de memoria
    // ------------------------------------------------------------
    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}

```
# `Slideshow/SlideshowViewModel.kt`


```kotlin
// Paquete donde se encuentra el ViewModel
// Relacionado con la sección "Slideshow" de la app
package com.example.appresponsive.ui.slideshow

// Importaciones necesarias para usar ViewModel y LiveData
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel

// ------------------------------------------------------------
// Clase SlideshowViewModel
// Mantiene los datos que la UI del SlideshowFragment mostrará
// ------------------------------------------------------------
class SlideshowViewModel : ViewModel() {

    // Variable privada de tipo MutableLiveData
    // Permite modificar el valor internamente
    private val _text = MutableLiveData<String>().apply {
        // Valor inicial que se mostrará en la pantalla del fragmento Slideshow
        value = "This is slideshow Fragment"
    }

    // Exposición del LiveData público (solo lectura) para la UI
    // Permite que el fragmento observe los cambios sin modificar el valor
    val text: LiveData<String> = _text
}
```
## En resumen

| Elemento | Función |
|-----------|----------|
| **ViewModel** | Mantiene los datos de la UI de forma persistente, incluso al rotar la pantalla. |
| **MutableLiveData** | Permite almacenar y modificar datos que la UI puede observar. |
| **LiveData (público)** | Permite que la UI observe los datos sin poder modificarlos directamente. |
| **apply { value = ... }** | Inicializa el valor de la variable al crear el ViewModel. |

# `transform/TransformFragment.kt`

```kotlin
// Paquete donde se encuentra este Fragment, usado para organizar el proyecto.
package com.example.appresponsive.ui.transform

// Importaciones necesarias para usar componentes de Android y Jetpack.
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ImageView
import android.widget.TextView
import androidx.core.content.res.ResourcesCompat
import androidx.fragment.app.Fragment
import androidx.lifecycle.ViewModelProvider
import androidx.recyclerview.widget.DiffUtil
import androidx.recyclerview.widget.ListAdapter
import androidx.recyclerview.widget.RecyclerView
import com.example.appresponsive.R
import com.example.appresponsive.databinding.FragmentTransformBinding
import com.example.appresponsive.databinding.ItemTransformBinding

/**
 * Fragment que demuestra un patrón de layout responsive.
 * Cambia la disposición de los items según el tamaño de la pantalla:
 * - LinearLayoutManager en pantallas pequeñas
 * - GridLayoutManager en pantallas grandes
 */
class TransformFragment : Fragment() {

    // Variable de binding para acceder a las vistas del layout
    private var _binding: FragmentTransformBinding? = null

    // Propiedad segura que solo es válida entre onCreateView y onDestroyView
    private val binding get() = _binding!!

    // Método llamado para crear la vista del Fragment
    override fun onCreateView(
        inflater: LayoutInflater,
        container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        // Obtenemos el ViewModel asociado a este Fragment
        val transformViewModel = ViewModelProvider(this).get(TransformViewModel::class.java)

        // Inflamos el layout usando ViewBinding
        _binding = FragmentTransformBinding.inflate(inflater, container, false)
        val root: View = binding.root

        // Configuramos el RecyclerView con su adapter
        val recyclerView = binding.recyclerviewTransform
        val adapter = TransformAdapter()
        recyclerView.adapter = adapter

        // Observamos los datos del ViewModel y actualizamos la lista cuando cambian
        transformViewModel.texts.observe(viewLifecycleOwner) {
            adapter.submitList(it)
        }

        // Devolvemos la vista raíz
        return root
    }

    // Limpiamos el binding para evitar fugas de memoria
    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }

    /**
     * Adapter para el RecyclerView que muestra los items del transform.
     * Extiende ListAdapter para manejar eficientemente los cambios en la lista.
     */
    class TransformAdapter :
        ListAdapter<String, TransformViewHolder>(object : DiffUtil.ItemCallback<String>() {

            // Compara si los items son el mismo
            override fun areItemsTheSame(oldItem: String, newItem: String): Boolean =
                oldItem == newItem

            // Compara si el contenido de los items es el mismo
            override fun areContentsTheSame(oldItem: String, newItem: String): Boolean =
                oldItem == newItem
        }) {

        // Lista de imágenes que se usarán en cada item
        private val drawables = listOf(
            R.drawable.avatar_1,
            R.drawable.avatar_2,
            R.drawable.avatar_3,
            R.drawable.avatar_4,
            R.drawable.avatar_5,
            R.drawable.avatar_6,
            R.drawable.avatar_7,
            R.drawable.avatar_8,
            R.drawable.avatar_9,
            R.drawable.avatar_10,
            R.drawable.avatar_11,
            R.drawable.avatar_12,
            R.drawable.avatar_13,
            R.drawable.avatar_14,
            R.drawable.avatar_15,
            R.drawable.avatar_16,
        )

        // Crea un ViewHolder inflando el layout de cada item
        override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): TransformViewHolder {
            val binding = ItemTransformBinding.inflate(LayoutInflater.from(parent.context))
            return TransformViewHolder(binding)
        }

        // Asocia los datos a cada ViewHolder
        override fun onBindViewHolder(holder: TransformViewHolder, position: Int) {
            // Asigna el texto del item
            holder.textView.text = getItem(position)

            // Asigna la imagen correspondiente
            holder.imageView.setImageDrawable(
                ResourcesCompat.getDrawable(holder.imageView.resources, drawables[position], null)
            )
        }
    }

    /**
     * ViewHolder que representa cada item en el RecyclerView.
     * Contiene referencias a los elementos de la UI del item.
     */
    class TransformViewHolder(binding: ItemTransformBinding) :
        RecyclerView.ViewHolder(binding.root) {

        val imageView: ImageView = binding.imageViewItemTransform
        val textView: TextView = binding.textViewItemTransform
    }
}

```

### Resumen de funcionamiento:

- `TransformFragment` usa un `RecyclerView` para mostrar una lista de nombres con imágenes.

- Cambia su diseño según el tamaño de la pantalla (`linear` o `grid`, definido fuera de este código).

- TransformAdapter administra la lista usando `ListAdapter` y `DiffUtil` para eficiencia.

- Cada item está representado por `TransformViewHolder` que vincula un `TextView` y un `ImageView`.


```kotlin
// Paquete donde se encuentra este ViewModel, usado para organizar el proyecto
package com.example.appresponsive.ui.transform

// Importaciones necesarias para usar LiveData y ViewModel
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel

/**
 * ViewModel asociado al TransformFragment.
 * Su función principal es mantener los datos de la UI de forma persistente,
 * incluso cuando el Fragment se recrea (por ejemplo, al rotar la pantalla).
 */
class TransformViewModel : ViewModel() {

    // MutableLiveData privado para mantener la lista de textos
    // MutableLiveData permite modificar los datos internamente
    private val _texts = MutableLiveData<List<String>>().apply {
        // Inicializamos la lista con 16 elementos: "This is item #1" ... "This is item #16"
        value = (1..16).mapIndexed { _, i ->
            "This is item # $i"
        }
    }

    // LiveData público que expone los datos a la UI sin permitir modificarlos desde fuera
    val texts: LiveData<List<String>> = _texts
}
```

### Resumen de funcionamiento:

- `_texts` es mutable internamente y almacena la lista de items.

- `texts` es la versión pública, inmutable para la UI.

- Esto asegura que la UI pueda **observar cambios** en los datos sin poder modificarlos directamente.

- Inicializa automáticamente 16 elementos con nombres tipo `"This is item # X"`.