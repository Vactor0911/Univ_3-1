#Menu
Ch07_ContextMenu

#### 실행 화면
![[Ch07_ContextMenu_1.png]]![[Ch07_ContextMenu_2.png]]
- 텍스트를 길게 클릭하면 콘텍스트 메뉴가 표시되고, 버튼을 클릭하면 해당하는 색상으로 배경 색이 변한다.
#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <TextView
        android:id="@+id/txt"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!\nThis Message is for Test"
        android:textSize="50sp"
        android:textStyle="bold"
        android:textColor="@color/black"/>

</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch07_contextmenu;

import android.graphics.Color;
import android.os.Bundle;
import android.view.ContextMenu;
import android.view.MenuItem;
import android.view.View;
import android.widget.TextView;

import androidx.activity.EdgeToEdge;
import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {
    TextView tv;

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

        tv = findViewById(R.id.txt);
        registerForContextMenu(tv);
    }

    @Override
    public void onCreateContextMenu(ContextMenu menu, View v, ContextMenu.ContextMenuInfo menuInfo) {
        super.onCreateContextMenu(menu, v, menuInfo);
        menu.setHeaderTitle("ContextMenu");
        menu.add(0, 1, 0, "red");
        menu.add(0, 2, 0, "green");
        menu.add(0, 3, 0, "blue");
    }

    @Override
    public boolean onContextItemSelected(@NonNull MenuItem item) {
        int id = item.getItemId();
        switch (id) {
            case 1:
                tv.setBackgroundColor(Color.RED);
                return true;
            case 2:
                tv.setBackgroundColor(Color.GREEN);
                return true;
            case 3:
                tv.setBackgroundColor(Color.BLUE);
                return true;
        }
        return false;
    }
}
```

#### values\themes.xml
```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Base.Theme.Ch07_ContextMenu" parent="Theme.Material3.DayNight.NoActionBar">
        <!-- Customize your light theme here. -->
        <!-- <item name="colorPrimary">@color/my_light_primary</item> -->
    </style>

    <style name="Theme.Ch07_ContextMenu" parent="Base.Theme.Ch07_ContextMenu" />
</resources>
```