#Weight #Widget #Color
Week06_Question01

#### 실행 화면
![[Week06_Question01.png]]
- 1 : 1 비율로 나눠진 각 패널이 존재하고, 파란색 패널의 오른쪽 아래로 정렬된 \[Button\]이 있다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <android.widget.Button  
        android:layout_width="match_parent"  
        android:layout_height="0dp"  
        android:background="#FF0000"  
        android:layout_weight="1"/>  
  
    <LinearLayout  
        android:layout_width="match_parent"  
        android:layout_height="0dp"  
        android:layout_weight="1"  
        android:orientation="horizontal">  
  
        <android.widget.Button  
            android:layout_width="0dp"  
            android:layout_height="match_parent"  
            android:layout_weight="1"  
            android:background="#00FF00" />  
  
        <LinearLayout  
            android:layout_width="0dp"  
            android:layout_height="match_parent"  
            android:layout_weight="1"  
            android:gravity="right|bottom"  
            android:background="#0000FF">  
  
            <Button  
                android:id="@+id/btn01"  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
  
        </LinearLayout>  
  
    </LinearLayout>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.week06_question01;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.os.Bundle;  
import android.util.Log;  
import android.view.View;  
import android.widget.Button;  
  
public class MainActivity extends AppCompatActivity {  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
  
        Button btn = findViewById(R.id.btn01);  
        btn.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View view) {  
                Log.d("Button", "Button Clicked!");  
            }        });    }}
```