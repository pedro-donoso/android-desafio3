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
