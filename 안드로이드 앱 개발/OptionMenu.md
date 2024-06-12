#Menu 
Ch07_OptionMenu

#### 실행 화면
![[OptionMenu_1.png|200]]![[OptionMenu_2.png|200]]![[OptionMenu_3.png|200]]![[OptionMenu_4.png|300]]
- 우측 상단에 메뉴가 표시되면 공간이 없는 메뉴는 접힌 상태로 표시된다.
- 메뉴를 클릭하면 배경 색상이 선택한 색상으로 변경된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        android:textSize="30sp"
        android:textStyle="bold"/>

</LinearLayout>
```

#### my_menu.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/red"
        android:icon="@android:drawable/ic_btn_speak_now"
        android:title="red"
        app:showAsAction="never"/>
    <item
        android:id="@+id/green"
        android:icon="@android:drawable/star_big_on"
        android:title="green"
        app:showAsAction="ifRoom"/>
    <item
        android:id="@+id/blue"
        android:icon="@android:drawable/checkbox_on_background"
        android:title="blue"
        app:showAsAction="always"/>

</menu>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch07_optionmenu;

import android.graphics.Color;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;

import androidx.activity.EdgeToEdge;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {
    View view;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });

        view = findViewById(R.id.main);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.my_menu, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(@NonNull MenuItem item) {
        int id = item.getItemId();

        if (id == R.id.red) {
            view.setBackgroundColor(Color.RED);
            return true;
        }
        else if (id == R.id.green) {
            view.setBackgroundColor(Color.GREEN);
            return true;
        }
        else if (id == R.id.blue) {
            view.setBackgroundColor(Color.BLUE);
            return true;
        }
        return false;
    }
}
```

#### themes.xml
```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Base.Theme.Ch07_OptionMenu" parent="Theme.Material3.DayNight">
        <!-- Customize your light theme here. -->
        <!-- <item name="colorPrimary">@color/my_light_primary</item> -->
    </style>

    <style name="Theme.Ch07_OptionMenu" parent="Base.Theme.Ch07_OptionMenu" />
</resources>
```