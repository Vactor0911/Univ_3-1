#Intent
Ch06_Intent

#### 실행 화면
- 1번 화면에서 아이디와 비밀번호를 입력한 후 버튼을 클릭하면 2번 화면에 1에서 넘어온 아이디와 비밀번호의 데이터가 표시된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <EditText  
        android:id="@+id/etId"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"/>  
    <EditText  
        android:id="@+id/etPassword"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"/>  
    <Button  
        android:id="@+id/btnLogin"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Login"/>  
    <TextView  
        android:id="@+id/tvText"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"/>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch06_intent;  
  
import androidx.activity.result.ActivityResultLauncher;  
import androidx.activity.result.contract.ActivityResultContracts;  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.app.Activity;  
import android.content.Intent;  
import android.os.Bundle;  
import android.util.Log;  
import android.view.View;  
import android.widget.Button;  
import android.widget.EditText;  
import android.widget.TextView;  
  
public class MainActivity extends AppCompatActivity {  
    EditText etId, etPassword;  
    TextView tv;  
    ActivityResultLauncher<Intent> launcher;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
  
        etId = findViewById(R.id.etId);  
        etPassword = findViewById(R.id.etPassword);  
        tv = findViewById(R.id.tvText);  
  
        Button btn = findViewById(R.id.btnLogin);  
        btn.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View view) {  
                Intent intent = new Intent(MainActivity.this, MainActivity2.class);  
                intent.putExtra("strId", etId.getText().toString() );  
                intent.putExtra("strPassword", etPassword.getText().toString() );  
                //startActivity(intent);  
                launcher.launch(intent);  
            }        });  
        launcher = registerForActivityResult(  
                new ActivityResultContracts.StartActivityForResult(),  
                result->{  
                    if (result.getResultCode() == Activity.RESULT_OK) {  
                        Intent data = result.getData();  
                        tv.setText( data.getStringExtra("strStatus") );  
                    }                });    }}
```

#### activity_main2.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <TextView  
        android:id="@+id/tvId"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"/>  
    <TextView  
        android:id="@+id/tvPassword"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"/>  
    <Button  
        android:id="@+id/btnExit"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Exit"/>  
  
</LinearLayout>
```

#### MainActivity2.java
```java
package kr.ac.mokwon.ch06_intent;  
  
import androidx.activity.result.ActivityResultLauncher;  
import androidx.activity.result.contract.ActivityResultContracts;  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.app.Activity;  
import android.content.Intent;  
import android.os.Bundle;  
import android.util.Log;  
import android.view.View;  
import android.widget.Button;  
import android.widget.TextView;  
  
public class MainActivity2 extends AppCompatActivity {  
    TextView tvId, tvPassword;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main2);  
  
        tvId = findViewById(R.id.tvId);  
        tvPassword = findViewById(R.id.tvPassword);  
  
        Intent intent = getIntent();  
        if (intent != null) {  
            tvId.setText( intent.getStringExtra("strId") );  
            tvPassword.setText( intent.getStringExtra("strPassword") );  
        }  
        Button btn = findViewById(R.id.btnExit);  
        btn.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View view) {  
                Intent intent = new Intent(MainActivity2.this, MainActivity.class);  
                intent.putExtra("strStatus", "로그인 성공!");  
                setResult(RESULT_OK, intent);  
                finish();  
            }        });    }}
```