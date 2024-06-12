#Dialog 
Week11_Question01

#### 실행 화면
![[Login&RegisterForm_1.png|200]]![[Login&RegisterForm_2.png|200]]![[Login&RegisterForm_3.png|200]]![[Login&RegisterForm_4.png|200]]
- 간단한 로그인과 회원가입을 구현한 앱
- `회원가입` 버튼을 누르면 커스텀 대화상자로 구성한 회원가입 폼이 표시된다.
- 아이디와 비밀번호를 입력하고 `OK` 버튼을 클릭하면 메인 메뉴에서 Toast로 정보가 출력된다.
- 등록한 회원 정보로 로그인을 시도하면 Toast로 로그인 성공 여부가 출력된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center"
    android:layout_margin="20dp">

    <LinearLayout
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="10dp">

        <TextView
            android:id="@+id/tvId"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="ID : "
            android:textSize="16sp"
            android:textStyle="bold"
            android:layout_weight="1"
            android:textAlignment="center"/>
        <EditText
            android:id="@+id/etId"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="2"/>

    </LinearLayout>

    <LinearLayout
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="10dp">

        <TextView
            android:id="@+id/tvPassword"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="Password : "
            android:textSize="16sp"
            android:textStyle="bold"
            android:layout_weight="1"
            android:textAlignment="center"/>
        <EditText
            android:id="@+id/etPassword"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="2"/>

    </LinearLayout>

    <LinearLayout
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="10dp">

        <Button
            android:id="@+id/btnLogin"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_margin="10dp"
            android:layout_weight="1"
            android:text="로그인"/>
        <Button
            android:id="@+id/btnRegister"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_margin="10dp"
            android:layout_weight="1"
            android:text="회원가입"/>

    </LinearLayout>

</LinearLayout>
```

#### custom_dialog.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_margin="10dp">

    <EditText
        android:id="@+id/etId"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="10dp"
        android:hint="Id"/>
    <EditText
        android:id="@+id/etPassword"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="10dp"
        android:hint="Password"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="10dp">

        <Button
            android:id="@+id/btnOk"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="OK"
            android:layout_margin="10dp"
            android:layout_weight="1"/>
        <Button
            android:id="@+id/btnCancel"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:text="Cancel"
            android:textSize="12sp"
            android:layout_margin="10dp"
            android:layout_weight="1"/>

    </LinearLayout>

</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.week11_question01;

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

import java.util.HashMap;

public class MainActivity extends AppCompatActivity {
    private HashMap<String, String> dictAccount = new HashMap<>();

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

        EditText etId = findViewById(R.id.etId);
        EditText etPassword = findViewById(R.id.etPassword);
        Button btnLogin = findViewById(R.id.btnLogin);
        Button btnRegister = findViewById(R.id.btnRegister);

        btnLogin.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                String strId = etId.getText().toString();
                String strPassword = etPassword.getText().toString();

                if ( strId.isEmpty() || strPassword.isEmpty() ) {
                    return;
                }

                try {
                    if ( dictAccount.get(strId).equals(strPassword) ) {
                        Toast.makeText(MainActivity.this, strId + "님 환영합니다.", Toast.LENGTH_SHORT).show();
                        return;
                    }
                }
                catch (Exception e) { }
                Toast.makeText(MainActivity.this, "로그인에 실패하였습니다.", Toast.LENGTH_SHORT).show();
            }
        });
        btnRegister.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Dialog dialog = new Dialog(MainActivity.this);
                dialog.setContentView(R.layout.custom_dialog);
                dialog.setTitle("register");

                final EditText etId = dialog.findViewById(R.id.etId);
                final EditText etPassword = dialog.findViewById(R.id.etPassword);
                final Button btnOk = dialog.findViewById(R.id.btnOk);
                final Button btnCancel = dialog.findViewById(R.id.btnCancel);

                btnOk.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View view) {
                        String strId = etId.getText() + "";
                        if ( dictAccount.containsKey(strId) ) {
                            Toast.makeText(MainActivity.this, "이미 가입된 사용자입니다.", Toast.LENGTH_SHORT).show();
                            dialog.dismiss();
                            return;
                        }

                        String strPassword = etPassword.getText() + "";
                        dictAccount.put(strId, strPassword);
                        Toast.makeText(MainActivity.this, strId + "님 가입 성공!", Toast.LENGTH_SHORT).show();
                        dialog.dismiss();
                    }
                });
                btnCancel.setOnClickListener(new View.OnClickListener() {
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