#ToggleButton #Switch
Ch05_ToggleButton

#### 실행 화면
![[Ch05_ToggleButton.png]]
- \[ToggleButton\]을 클릭하면 ON / OFF가 전환된다.
- \[ToggleButton\]의 값에 따라 배경의 색이 흰색 / 회색으로 바뀌고, \[TextView\]의 글자가 변경된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:id="@+id/layout01"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:gravity="center"  
    android:background="#444444">  
  
    <TextView  
        android:id="@+id/tv01"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="OFF"  
        android:textColor="#FFFFFF"  
        android:textSize="30sp"/>  
  
    <ToggleButton  
        android:id="@+id/tb01"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="ToggleButton"  
        android:onClick="OnToggleClick"/>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch05_togglebutton;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.graphics.Color;  
import android.os.Bundle;  
import android.view.View;  
import android.widget.LinearLayout;  
import android.widget.TextView;  
import android.widget.ToggleButton;  
  
public class MainActivity extends AppCompatActivity {  
    LinearLayout layout;  
    TextView tv;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
  
        layout = findViewById(R.id.layout01);  
        tv = findViewById(R.id.tv01);  
    }  
    public void OnToggleClick(View view) {  
        ToggleButton tb = (ToggleButton)view;  
  
        if ( tb.isChecked() ) {  
            layout.setBackgroundColor( Color.parseColor("#FFFFFF") );  
            tv.setText("ON");  
            tv.setTextColor( Color.parseColor("#444444") );  
        }        else {  
            layout.setBackgroundColor( Color.parseColor("#444444") );  
            tv.setText("OFF");  
            tv.setTextColor( Color.parseColor("#FFFFFF") );  
        }    }}
```