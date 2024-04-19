#Button #Visibility #OnClick #Logcat
Ch03_01
#### 실행 화면
![[Ch03_01.png]]
- 각 버튼을 클릭하면 버튼의 이름이 Logcat에 표시된다.
- Button02의 visibility를 invisible로 하여 화면에 표시되지 않는다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <TextView  
        android:id="@+id/text01"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Hello World!"  
        android:visibility="gone" />  
    <Button  
        android:id="@+id/btn01"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Button01" />  
    <Button  
        android:id="@+id/btn02"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Button02"  
        android:visibility="invisible" />  
    <Button  
        android:id="@+id/btn03"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Button03" />  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch03_01;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.os.Bundle;  
import android.util.Log;  
import android.view.View;  
import android.widget.Button;  
  
public class MainActivity extends AppCompatActivity implements View.OnClickListener {  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
  
        Button btn01 = findViewById(R.id.btn01);  
        Button btn02 = findViewById(R.id.btn02);  
        Button btn03 = findViewById(R.id.btn03);  
        btn01.setOnClickListener(this);  
        btn02.setOnClickListener(this);  
        btn03.setOnClickListener(this);  
    }  
    @Override  
    public void onClick(View view) {  
        Button btn = (Button)view;  
        switch( btn.getId() ){  
            case R.id.btn01:  
                Log.i("mokwon", "Button01");  
                break;  
            case R.id.btn02:  
                Log.i("mokwon", "Button02");  
                break;  
            case R.id.btn03:  
                Log.i("mokwon", "Button03");  
                break;  
        }    }}
```
