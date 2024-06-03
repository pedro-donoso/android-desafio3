# android-desafio3

1. Una actividad que contenga un NavHostFragment para mostrar los fragmentos.

#### build.gradle:
   ```
   implementation 'androidx.navigation:navigation-fragment-ktx:2.5.0'
   implementation 'androidx.navigation:navigation-ui-ktx:2.5.0'
   ```

#### activity_main.xml:
   ```
   <androidx.fragment.app.FragmentContainerView
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/nav_host_fragment"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:navGraph="@navigation/nav_graph"
    app:defaultNavHost="true"
    android:name="androidx.navigation.fragment.NavHostFragment" />
   ```

#### nav_graph.xml:
   ```
   <?xml version="1.0" encoding="utf-8"?>
    <navigation xmlns:android="http://schemas.android.com/apk/res/android"
      xmlns:app="http://schemas.android.com/apk/res-auto"
      xmlns:tools="http://schemas.android.com/tools"
      app:startDestination="@id/firstFragment">

      <fragment
        android:id="@+id/firstFragment"
        android:name="com.example.app.FirstFragment"
        android:label="First Fragment"
        tools:layout="@layout/fragment_first" >
        <action
            android:id="@+id/action_firstFragment_to_secondFragment"
            app:destination="@id/secondFragment" />
      </fragment>

    <fragment
        android:id="@+id/secondFragment"
        android:name="com.example.app.SecondFragment"
        android:label="Second Fragment"
        tools:layout="@layout/fragment_second" />
  </navigation>
   ```

#### Main_Activity.kt:
   ```
  import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.navigation.fragment.NavHostFragment
import androidx.navigation.ui.NavigationUI
import com.example.app.R

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val navHostFragment = supportFragmentManager
            .findFragmentById(R.id.nav_host_fragment) as NavHostFragment
        val navController = navHostFragment.navController

        // Configurar la barra de acción para usar NavController
        NavigationUI.setupActionBarWithNavController(this, navController)
    }

    override fun onSupportNavigateUp(): Boolean {
        val navHostFragment = supportFragmentManager
            .findFragmentById(R.id.nav_host_fragment) as NavHostFragment
        val navController = navHostFragment.navController
        return navController.navigateUp() || super.onSupportNavigateUp()
    }
}
```

2. La pantalla principal está definida en un fragmento donde el usuario ingresa la contraseña. El diseño de la pantalla es libre, pero debe incluir un campo de texto para ingresar la contraseña, un botón deshabilitado que se habilita cuando se cumplen los criterios y un texto indicando las características que debe tener la contraseña.

   #### fragment_main.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvPasswordCriteria"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="La contraseña debe tener al menos 8 caracteres, incluir una letra mayúscula, una letra minúscula y un número."
        android:paddingBottom="16dp" />

    <EditText
        android:id="@+id/etPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Ingrese su contraseña"
        android:inputType="textPassword" />

    <Button
        android:id="@+id/btnSubmit"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Enviar"
        android:enabled="false"
        android:layout_marginTop="16dp" />
</LinearLayout>
   ```

#### MainFragment.kt:

```
import android.os.Bundle
import android.text.Editable
import android.text.TextWatcher
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
android.widget.Button
android.widget.EditText
import androidx.fragment.app.Fragment
import com.example.app.R

class MainFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        val view = inflater.inflate(R.layout.fragment_main, container, false)

        val etPassword = view.findViewById<EditText>(R.id.etPassword)
        val btnSubmit = view.findViewById<Button>(R.id.btnSubmit)

        etPassword.addTextChangedListener(object : TextWatcher {
            override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {}

            override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {}

            override fun afterTextChanged(s: Editable?) {
                val password = s.toString()
                btnSubmit.isEnabled = isPasswordValid(password)
            }
        })

        btnSubmit.setOnClickListener {
            // Manejar el evento de clic del botón
        }

        return view
    }

    private fun isPasswordValid(password: String): Boolean {
        val passwordPattern = Regex("^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d).{8,}$")
        return password.matches(passwordPattern)
    }
}
```

#### nav_graph.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    app:startDestination="@id/mainFragment">

    <fragment
        android:id="@+id/mainFragment"
        android:name="com.example.app.MainFragment"
        android:label="Main Fragment"
        tools:layout="@layout/fragment_main" />
</navigation>
```

#### MainActivity.kt:
```
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.navigation.fragment.NavHostFragment
import androidx.navigation.ui.NavigationUI
import com.example.app.R

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val navHostFragment = supportFragmentManager
            .findFragmentById(R.id.nav_host_fragment) as NavHostFragment
        val navController = navHostFragment.navController

        NavigationUI.setupActionBarWithNavController(this, navController)
    }

    override fun onSupportNavigateUp(): Boolean {
        val navHostFragment = supportFragmentManager
            .findFragmentById(R.id.nav_host_fragment) as NavHostFragment
        val navController = navHostFragment.navController
        return navController.navigateUp() || super.onSupportNavigateUp()
    }
}
```
3. La pantalla de resultado está definida en un fragmento donde se le muestra al usuario los criterios usados para validar su contraseña, y algunos consejos para mejorar su seguridad.

   #### fragment_result.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvPasswordCriteria"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Criterios para validar la contraseña:"
        android:textStyle="bold"
        android:paddingBottom="8dp" />

    <TextView
        android:id="@+id/tvCriteria"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingBottom="16dp"
        android:text="• Al menos 8 caracteres\n• Una letra mayúscula\n• Una letra minúscula\n• Un número" />

    <TextView
        android:id="@+id/tvSecurityTips"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Consejos para mejorar la seguridad:"
        android:textStyle="bold"
        android:paddingBottom="8dp" />

    <TextView
        android:id="@+id/tvTips"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="• Usa una combinación de caracteres especiales\n• No uses palabras comunes o información personal\n• Cambia tu contraseña regularmente\n• Usa un gestor de contraseñas" />
</LinearLayout>
   ```

#### ResultFragment.kt:
```
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import com.example.app.R

class ResultFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_result, container, false)
    }
}
```

#### nav_graph.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    app:startDestination="@id/mainFragment">

    <fragment
        android:id="@+id/mainFragment"
        android:name="com.example.app.MainFragment"
        android:label="Main Fragment"
        tools:layout="@layout/fragment_main">
        <action
            android:id="@+id/action_mainFragment_to_resultFragment"
            app:destination="@id/resultFragment" />
    </fragment>

    <fragment
        android:id="@+id/resultFragment"
        android:name="com.example.app.ResultFragment"
        android:label="Result Fragment"
        tools:layout="@layout/fragment_result" />
</navigation>
```

#### MainFragment.kt:

```
import android.os.Bundle
import android.text.Editable
import android.text.TextWatcher
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
android.widget.Button
android.widget.EditText
import androidx.fragment.app.Fragment
import androidx.navigation.fragment.findNavController
import com.example.app.R

class MainFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        val view = inflater.inflate(R.layout.fragment_main, container, false)

        val etPassword = view.findViewById<EditText>(R.id.etPassword)
        val btnSubmit = view.findViewById<Button>(R.id.btnSubmit)

        etPassword.addTextChangedListener(object : TextWatcher {
            override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {}

            override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {}

            override fun afterTextChanged(s: Editable?) {
                val password = s.toString()
                btnSubmit.isEnabled = isPasswordValid(password)
            }
        })

        btnSubmit.setOnClickListener {
            // Navegar al fragmento de resultados
            findNavController().navigate(R.id.action_mainFragment_to_resultFragment)
        }

        return view
    }

    private fun isPasswordValid(password: String): Boolean {
        val passwordPattern = Regex("^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d).{8,}$")
        return password.matches(passwordPattern)
    }
}
```

4. Utilizar ViewBinding para enlazar y utilizar las vistas.

   #### buil.gradle:

```
android {
    ...
    viewBinding {
        enabled = true
    }
}
   ```

#### MainFragment.kt:

```
import android.os.Bundle
import android.text.Editable
import android.text.TextWatcher
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import androidx.navigation.fragment.findNavController
import com.example.app.databinding.FragmentMainBinding

class MainFragment : Fragment() {

    private var _binding: FragmentMainBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        _binding = FragmentMainBinding.inflate(inflater, container, false)
        val view = binding.root

        binding.etPassword.addTextChangedListener(object : TextWatcher {
            override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {}

            override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {}

            override fun afterTextChanged(s: Editable?) {
                val password = s.toString()
                binding.btnSubmit.isEnabled = isPasswordValid(password)
            }
        })

        binding.btnSubmit.setOnClickListener {
            findNavController().navigate(R.id.action_mainFragment_to_resultFragment)
        }

        return view
    }

    private fun isPasswordValid(password: String): Boolean {
        val passwordPattern = Regex("^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d).{8,}$")
        return password.matches(passwordPattern)
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

#### ResultFragment.kt:

```
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import com.example.app.databinding.FragmentResultBinding

class ResultFragment : Fragment() {

    private var _binding: FragmentResultBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        _binding = FragmentResultBinding.inflate(inflater, container, false)
        val view = binding.root

        // Configurar las vistas si es necesario

        return view
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

#### fragment_main.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvPasswordCriteria"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="La contraseña debe tener al menos 8 caracteres, incluir una letra mayúscula, una letra minúscula y un número."
        android:paddingBottom="16dp" />

    <EditText
        android:id="@+id/etPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Ingrese su contraseña"
        android:inputType="textPassword" />

    <Button
        android:id="@+id/btnSubmit"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Enviar"
        android:enabled="false"
        android:layout_marginTop="16dp" />
</LinearLayout>
```

#### fragment_result.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvPasswordCriteria"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Criterios para validar la contraseña:"
        android:textStyle="bold"
        android:paddingBottom="8dp" />

    <TextView
        android:id="@+id/tvCriteria"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingBottom="16dp"
        android:text="• Al menos 8 caracteres\n• Una letra mayúscula\n• Una letra minúscula\n• Un número" />

    <TextView
        android:id="@+id/tvSecurityTips"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Consejos para mejorar la seguridad:"
        android:textStyle="bold"
        android:paddingBottom="8dp" />

    <TextView
        android:id="@+id/tvTips"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="• Usa una combinación de caracteres especiales\n• No uses palabras comunes o información personal\n• Cambia tu contraseña regularmente\n• Usa un gestor de contraseñas" />
</LinearLayout>
```

5. Validación de la contraseña aplicando los 2 criterios: 
      a. Tiene más de 5 caracteres. 
      b. Tiene al menos una letra mayúscula.

#### MainFragment.kt:
```
private fun isPasswordValid(password: String): Boolean {
    val lengthCriteria = password.length > 5
    val uppercaseCriteria = password.any { it.isUpperCase() }
    return lengthCriteria && uppercaseCriteria
}
```

#### fragment_mail.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvPasswordCriteria"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="La contraseña debe tener más de 5 caracteres y al menos una letra mayúscula."
        android:paddingBottom="16dp" />

    <EditText
        android:id="@+id/etPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Ingrese su contraseña"
        android:inputType="textPassword" />

    <Button
        android:id="@+id/btnSubmit"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Enviar"
        android:enabled="false"
        android:layout_marginTop="16dp" />
</LinearLayout>
```

#### MainFragment.xml:

```
import android.os.Bundle
import android.text.Editable
import android.text.TextWatcher
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import androidx.navigation.fragment.findNavController
import com.example.app.databinding.FragmentMainBinding

class MainFragment : Fragment() {

    private var _binding: FragmentMainBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        _binding = FragmentMainBinding.inflate(inflater, container, false)
        val view = binding.root

        binding.etPassword.addTextChangedListener(object : TextWatcher {
            override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {}

            override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {}

            override fun afterTextChanged(s: Editable?) {
                val password = s.toString()
                binding.btnSubmit.isEnabled = isPasswordValid(password)
            }
        })

        binding.btnSubmit.setOnClickListener {
            findNavController().navigate(R.id.action_mainFragment_to_resultFragment)
        }

        return view
    }

    private fun isPasswordValid(password: String): Boolean {
        val lengthCriteria = password.length > 5
        val uppercaseCriteria = password.any { it.isUpperCase() }
        return lengthCriteria && uppercaseCriteria
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

#### fragment_result.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvPasswordCriteria"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Criterios para validar la contraseña:"
        android:textStyle="bold"
        android:paddingBottom="8dp" />

    <TextView
        android:id="@+id/tvCriteria"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingBottom="16dp"
        android:text="• Más de 5 caracteres\n• Al menos una letra mayúscula" />

    <TextView
        android:id="@+id/tvSecurityTips"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Consejos para mejorar la seguridad:"
        android:textStyle="bold"
        android:paddingBottom="8dp" />

    <TextView
        android:id="@+id/tvTips"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="• Usa una combinación de caracteres especiales\n• No uses palabras comunes o información personal\n• Cambia tu contraseña regularmente\n• Usa un gestor de contraseñas" />
</LinearLayout>
```

#### FragmentResult.xml:

```
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import com.example.app.databinding.FragmentResultBinding

class ResultFragment : Fragment() {

    private var _binding: FragmentResultBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        _binding = FragmentResultBinding.inflate(inflater, container, false)
        val view = binding.root

        return view
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

6. Utilizar un ChangeListener para escuchar los cambios en la contraseña y activar el botón cuando se cumplen las condiciones, pero cuando las condiciones no se cumplen, el botón debe estar deshabilitado.

#### MainFragment.kt:

```
import android.os.Bundle
import android.text.Editable
import android.text.TextWatcher
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import androidx.navigation.fragment.findNavController
import com.example.app.databinding.FragmentMainBinding

class MainFragment : Fragment() {

    private var _binding: FragmentMainBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        _binding = FragmentMainBinding.inflate(inflater, container, false)
        val view = binding.root

        binding.etPassword.addTextChangedListener(passwordTextWatcher)

        binding.btnSubmit.setOnClickListener {
            findNavController().navigate(R.id.action_mainFragment_to_resultFragment)
        }

        return view
    }

    private val passwordTextWatcher = object : TextWatcher {
        override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {}

        override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {}

        override fun afterTextChanged(s: Editable?) {
            val password = s.toString()
            binding.btnSubmit.isEnabled = isPasswordValid(password)
        }
    }

    private fun isPasswordValid(password: String): Boolean {
        val lengthCriteria = password.length > 5
        val uppercaseCriteria = password.any { it.isUpperCase() }
        return lengthCriteria && uppercaseCriteria
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

#### fragment_main.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvPasswordCriteria"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="La contraseña debe tener más de 5 caracteres y al menos una letra mayúscula."
        android:paddingBottom="16dp" />

    <EditText
        android:id="@+id/etPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Ingrese su contraseña"
        android:inputType="textPassword" />

    <Button
        android:id="@+id/btnSubmit"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Enviar"
        android:enabled="false"
        android:layout_marginTop="16dp" />
</LinearLayout>
```

7. Utilizar onClickListener para reaccionar al click en el botón.

#### MainFragment.kt
```
import android.os.Bundle
import android.text.Editable
import android.text.TextWatcher
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import androidx.navigation.fragment.findNavController
import com.example.app.databinding.FragmentMainBinding

class MainFragment : Fragment() {

    private var _binding: FragmentMainBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        _binding = FragmentMainBinding.inflate(inflater, container, false)
        val view = binding.root

        // Set up TextWatcher to listen for password changes
        binding.etPassword.addTextChangedListener(passwordTextWatcher)

        // Set up OnClickListener for the button
        binding.btnSubmit.setOnClickListener {
            onSubmitButtonClick()
        }

        return view
    }

    private val passwordTextWatcher = object : TextWatcher {
        override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {}

        override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {}

        override fun afterTextChanged(s: Editable?) {
            val password = s.toString()
            binding.btnSubmit.isEnabled = isPasswordValid(password)
        }
    }

    private fun isPasswordValid(password: String): Boolean {
        val lengthCriteria = password.length > 5
        val uppercaseCriteria = password.any { it.isUpperCase() }
        return lengthCriteria && uppercaseCriteria
    }

    private fun onSubmitButtonClick() {
        // Navigate to the result fragment
        findNavController().navigate(R.id.action_mainFragment_to_resultFragment)
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

8. Utilizar Navigation para navegar entre fragmentos, donde debe estar definido el NavHostFragment y el grafo de navegación.

#### activity_main.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <fragment
        android:id="@+id/nav_host_fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:navGraph="@navigation/nav_graph"
        app:defaultNavHost="true" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

#### nav_graph.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/mainFragment">

    <fragment
        android:id="@+id/mainFragment"
        android:name="com.example.app.MainFragment"
        android:label="MainFragment"
        tools:layout="@layout/fragment_main" >
        <action
            android:id="@+id/action_mainFragment_to_resultFragment"
            app:destination="@id/resultFragment" />
    </fragment>

    <fragment
        android:id="@+id/resultFragment"
        android:name="com.example.app.ResultFragment"
        android:label="ResultFragment"
        tools:layout="@layout/fragment_result" />
</navigation>
```

#### MainActivity.kt:
```
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.navigation.NavController
import androidx.navigation.fragment.NavHostFragment
import androidx.navigation.ui.setupActionBarWithNavController
import com.example.app.R

class MainActivity : AppCompatActivity() {

    private lateinit var navController: NavController

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val navHostFragment = supportFragmentManager
            .findFragmentById(R.id.nav_host_fragment) as NavHostFragment
        navController = navHostFragment.navController

        setupActionBarWithNavController(navController)
    }

    override fun onSupportNavigateUp(): Boolean {
        return navController.navigateUp() || super.onSupportNavigateUp()
    }
}
```

#### MainFragment.kt:

```
// MainFragment.kt
import android.os.Bundle
import android.text.Editable
import android.text.TextWatcher
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import androidx.navigation.fragment.findNavController
import com.example.app.databinding.FragmentMainBinding

class MainFragment : Fragment() {

    private var _binding: FragmentMainBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        _binding = FragmentMainBinding.inflate(inflater, container, false)
        val view = binding.root

        // Set up TextWatcher to listen for password changes
        binding.etPassword.addTextChangedListener(passwordTextWatcher)

        // Set up OnClickListener for the button
        binding.btnSubmit.setOnClickListener {
            onSubmitButtonClick()
        }

        return view
    }

    private val passwordTextWatcher = object : TextWatcher {
        override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {}

        override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {}

        override fun afterTextChanged(s: Editable?) {
            val password = s.toString()
            binding.btnSubmit.isEnabled = isPasswordValid(password)
        }
    }

    private fun isPasswordValid(password: String): Boolean {
        val lengthCriteria = password.length > 5
        val uppercaseCriteria = password.any { it.isUpperCase() }
        return lengthCriteria && uppercaseCriteria
    }

    private fun onSubmitButtonClick() {
        // Navigate to the result fragment
        findNavController().navigate(R.id.action_mainFragment_to_resultFragment)
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

#### ResultFragment.kt:

```
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import com.example.app.databinding.FragmentResultBinding

class ResultFragment : Fragment() {

    private var _binding: FragmentResultBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        _binding = FragmentResultBinding.inflate(inflater, container, false)
        val view = binding.root

        // Configure views if needed

        return view
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

9. Navegar hacia la pantalla de resultado.

#### nav_graph.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/mainFragment">

    <fragment
        android:id="@+id/mainFragment"
        android:name="com.example.app.MainFragment"
        android:label="MainFragment"
        tools:layout="@layout/fragment_main" >
        <action
            android:id="@+id/action_mainFragment_to_resultFragment"
            app:destination="@id/resultFragment" />
    </fragment>

    <fragment
        android:id="@+id/resultFragment"
        android:name="com.example.app.ResultFragment"
        android:label="ResultFragment"
        tools:layout="@layout/fragment_result" />
</navigation>
```

#### activity_main.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <fragment
        android:id="@+id/nav_host_fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:navGraph="@navigation/nav_graph"
        app:defaultNavHost="true" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

#### MainActivity.kt:

```
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.navigation.NavController
import androidx.navigation.fragment.NavHostFragment
import androidx.navigation.ui.setupActionBarWithNavController
import com.example.app.R

class MainActivity : AppCompatActivity() {

    private lateinit var navController: NavController

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val navHostFragment = supportFragmentManager
            .findFragmentById(R.id.nav_host_fragment) as NavHostFragment
        navController = navHostFragment.navController

        setupActionBarWithNavController(navController)
    }

    override fun onSupportNavigateUp(): Boolean {
        return navController.navigateUp() || super.onSupportNavigateUp()
    }
}
```

#### MainFragment.kt:

```
import android.os.Bundle
import android.text.Editable
import android.text.TextWatcher
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import androidx.navigation.fragment.findNavController
import com.example.app.databinding.FragmentMainBinding

class MainFragment : Fragment() {

    private var _binding: FragmentMainBinding? = null
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        _binding = FragmentMainBinding.inflate(inflater, container, false)
        val view = binding.root

        // Set up TextWatcher to listen for password changes
        binding.etPassword.addTextChangedListener(passwordTextWatcher)

        // Set up OnClickListener for the button
        binding.btnSubmit.setOnClickListener {
            onSubmitButtonClick()
        }

        return view
    }

    private val passwordTextWatcher = object : TextWatcher {
        override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {}

        override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {}

        override fun afterTextChanged(s: Editable?) {
            val password = s.toString()
            binding.btnSubmit.isEnabled = isPasswordValid(password)
        }
    }

    private fun isPasswordValid(password: String): Boolean {
        val lengthCriteria = password.length > 5
        val uppercaseCriteria = password.any { it.isUpperCase() }
        return lengthCriteria && uppercaseCriteria
    }

    private fun onSubmitButtonClick() {
        // Navigate to the result fragment
        findNavController().navigate(R.id.action_mainFragment_to_resultFragment)
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

10.  Extraer los textos al archivo de recursos de textos (strings.xml):

#### strings.xml:
```
<resources>
    <string name="app_name">YourAppName</string>
    <string name="password_criteria">La contraseña debe tener más de 5 caracteres y al menos una letra mayúscula.</string>
    <string name="password_hint">Ingrese su contraseña</string>
    <string name="submit_button_text">Enviar</string>
    <string name="main_fragment_label">MainFragment</string>
    <string name="result_fragment_label">ResultFragment</string>
</resources>
```

#### fragment_main.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvPasswordCriteria"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/password_criteria"
        android:paddingBottom="16dp" />

    <EditText
        android:id="@+id/etPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="@string/password_hint"
        android:inputType="textPassword" />

    <Button
        android:id="@+id/btnSubmit"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/submit_button_text"
        android:enabled="false"
        android:layout_marginTop="16dp" />
</LinearLayout>
```

#### nav_graph.xml:

```
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/mainFragment">

    <fragment
        android:id="@+id/mainFragment"
        android:name="com.example.app.MainFragment"
        android:label="@string/main_fragment_label"
        tools:layout="@layout/fragment_main" >
        <action
            android:id="@+id/action_mainFragment_to_resultFragment"
            app:destination="@id/resultFragment" />
    </fragment>

    <fragment
        android:id="@+id/resultFragment"
        android:name="com.example.app.ResultFragment"
        android:label="@string/result_fragment_label"
        tools:layout="@layout/fragment_result" />
</navigation>
```
