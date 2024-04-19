#EditText #TextView
Ch03_03

#### 실행 화면
![[Ch03_03.png]]
- 아이디와 패스워드를 입력하고 회원가입 버튼을 클릭하면 아래에 아이디와 비밀번호가 표시된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <EditText  
        android:id="@+id/editId"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:padding="10dp"  
        android:layout_margin="10dp"  
        android:hint="아이디를 입력하세요"/>  
  
    <EditText  
        android:id="@+id/editPass"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:padding="10dp"  
        android:layout_margin="10dp"  
        android:hint="패스워드를 입력하세요"  
        android:inputType="textPassword"/>  
  
    <Button  
        android:id="@+id/btnRegister"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_margin="10dp"  
        android:text="회원가입"/>  
  
    <TextView  
        android:id="@+id/tvResult"  
        android:layout_width="match_parent"  
        android:layout_height="match_parent"  
        android:layout_margin="10dp"/>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch03_03;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.os.Bundle;  
import android.view.View;  
import android.widget.Button;  
import android.widget.EditText;  
import android.widget.TextView;  
  
public class MainActivity extends AppCompatActivity {  
    private EditText _idText, _passText;  
    private TextView _text;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
        _idText = findViewById(R.id.editId);  
        _passText = findViewById(R.id.editPass);  
        _text = findViewById(R.id.tvResult);  
  
        Button btn = findViewById(R.id.btnRegister);  
        btn.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View view) {  
                String _strData = "아이디 : " + _idText.getText() + "\n패스워드 : " + _passText.getText();  
                _text.setText(_strData);  
                _idText.setText("");  
                _passText.setText("");  
            }        });    }}
```