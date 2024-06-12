#Menu 
Ch07_PopupMenu

#### 실행 화면
![[PopupMenu_1.png]]![[PopupMenu_2.png]]
- `Hello World!` 버튼을 클릭하면 팝업 메뉴가 표시된다.
- 팝업 메뉴의 버튼을 클릭하면 해당하는 버튼의 텍스트가 Toast로 표시된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <Button
        android:id="@+id/btn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        android:textSize="20sp"/>

</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch07_popupmenu;

import android.os.Bundle;
import android.view.MenuItem;
import android.view.View;
import android.widget.Button;
import android.widget.PopupMenu;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {

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

        Button btn = findViewById(R.id.btn);
        btn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                PopupMenu popcupMenu = new PopupMenu(MainActivity.this, view);
                popupMenu.getMenuInflater().inflate( R.menu.my_menu, popupMenu.getMenu() );
                popupMenu.setOnMenuItemClickListener(new PopupMenu.OnMenuItemClickListener() {
                    @Override
                    public boolean onMenuItemClick(MenuItem menuItem) {
                        Toast.makeText(MainActivity.this, "Clicked Data : " + menuItem.getTitle(), Toast.LENGTH_SHORT).show();
                        return true;
                    }
                });
                popupMenu.show();
            }
        });
    }
}
```

#### my_menu.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">

    <item
        android:id="@+id/search"
        android:icon="@android:drawable/ic_menu_search"
        android:title="search"/>
    <item
        android:id="@+id/add"
        android:icon="@android:drawable/ic_menu_add"
        android:title="add"/>
    <item
        android:id="@+id/edit"
        android:icon="@android:drawable/ic_menu_edit"
        android:title="edit">
        <menu>
            <item
                android:id="@+id/share"
                android:icon="@android:drawable/ic_menu_share"
                android:title="share"/>
        </menu>
    </item>

</menu>
```