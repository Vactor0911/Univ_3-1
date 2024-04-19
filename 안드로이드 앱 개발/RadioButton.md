#RadioButton
Ch05_RadioButton

#### 실행 화면
![[Ch05_RadioButton.png]]
- 라디오 버튼을 선택하면 하단의 \[TextView\]에 선택한 라디오 버튼의 이름이 표시된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <RadioGroup  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:orientation="vertical">  
  
        <RadioButton  
            android:id="@+id/rb01"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:text="Red"  
            android:onClick="OnRadioClick"/>  
        <RadioButton  
            android:id="@+id/rb02"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:text="Green"  
            android:onClick="OnRadioClick"/>  
        <RadioButton  
            android:id="@+id/rb03"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:text="Blue"  
            android:onClick="OnRadioClick"/>  
  
    </RadioGroup>  
  
    <TextView  
        android:id="@+id/tv01"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:text="Selected : "  
        android:textSize="20sp"/>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch05_radiobutton;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.os.Bundle;  
import android.util.Log;  
import android.view.View;  
import android.widget.RadioButton;  
import android.widget.TextView;  
  
public class MainActivity extends AppCompatActivity {  
    TextView tv;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
  
        tv = findViewById(R.id.tv01);  
    }  
    public void OnRadioClick(View view) {  
        RadioButton rb = (RadioButton)view;  
        if ( !rb.isChecked() ) {  
            return;  
        }        String str = "Selected : " + rb.getText();  
        tv.setText(str);  
    }}
```