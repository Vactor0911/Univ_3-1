#Switch
Ch05_ToggleTest

#### 실행 화면
![[Ch05_ToggleTest.png]]
- 두 개의 버튼을 XOR 연산한 결과를 아래 \[Switch\]의 켜짐 / 꺼짐으로 표현한다.
	
	| 버튼 1 | 버튼 2 | 스위치 |
	| --- | --- | --- |
	| OFF | OFF | OFF |
	| ON | OFF | ON |
	| OFF | ON | ON |
	| ON | ON | OFF |

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <ToggleButton  
        android:id="@+id/tb01"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="ON"/>  
    <ToggleButton  
        android:id="@+id/tb02"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="OFF"/>  
    <Switch  
        android:id="@+id/s01"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:clickable="false"/>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch05_toggletest;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.os.Bundle;  
import android.view.View;  
import android.widget.Switch;  
import android.widget.ToggleButton;  
  
public class MainActivity extends AppCompatActivity implements View.OnClickListener {  
    ToggleButton tb1, tb2;  
    Switch s1;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
  
        tb1 = findViewById(R.id.tb01);  
        tb2 = findViewById(R.id.tb02);  
        s1 = findViewById(R.id.s01);  
  
        tb1.setOnClickListener(this);  
        tb2.setOnClickListener(this);  
    }  
    @Override  
    public void onClick(View view) {  
        s1.setChecked( (tb1.isChecked() != tb2.isChecked() ? true : false) );  
    }}
```