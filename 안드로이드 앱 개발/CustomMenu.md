#Menu 
Ch07_CustomMenu

#### 실행 화면
![[Ch07_CustomMenu_1.png|200]]![[Ch07_CustomMenu_2.png|200]]![[Ch07_CustomMenu_3.png|200]]
- 메인 화면의 `Hello World!` 버튼을 클릭하면 `아이디`
와 `비밀번호`를 입력할 수 있는 폼이 출력된다.
- 입력 폼에서 `Yes` 버튼을 클릭하면 메인 화면에 Toast로 입력한 정보가 출력된다.

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
        android:textSize="30sp"/>

</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch07_custommenu;

import android.app.Dialog;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
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
                Dialog dialog = new Dialog(MainActivity.this);
                dialog.setContentView(R.layout.custom_dialog);
                dialog.setTitle("login");

                final EditText etName = dialog.findViewById(R.id.etName);
                final EditText etPassword = dialog.findViewById(R.id.etPassword);
                final Button btnYes = dialog.findViewById(R.id.btnYes);
                final Button btnNo = dialog.findViewById(R.id.btnNo);

                btnYes.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        String strMessage = etName.getText() + "\n" + etPassword.getText();
                        Toast.makeText(MainActivity.this, strMessage, Toast.LENGTH_SHORT).show();
                        dialog.dismiss();
                    }
                });
                btnNo.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        dialog.dismiss();
                    }
                });

                dialog.show();
            }
        });
    }
}
```

#### custom_dialog.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_margin="20dp">

    <EditText
        android:id="@+id/etName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="10dp"
        android:hint="User Name"/>
    <EditText
        android:id="@+id/etPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="10dp"
        android:inputType="textPassword"
        android:hint="Password"/>

    <LinearLayout
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <Button
            android:id="@+id/btnYes"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="Yes"
            android:layout_weight="1"
            android:layout_margin="10dp"
            android:textSize="20sp"/>
        <Button
            android:id="@+id/btnNo"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="No"
            android:layout_weight="1"
            android:layout_margin="10dp"
            android:textSize="20sp"/>

    </LinearLayout>

</LinearLayout>
```