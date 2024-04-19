#CheckBox #CheckClick
Ch05_CompoundButton

#### 실행 화면
![[Ch05_CompoundButton.png]]
- 체크박스를 체크할 때마다 하단의 \[TextView\]가 업데이트 되며, 체크된 체크박스의 목록을 표시한다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <CheckBox  
        android:id="@+id/cb01"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Check01"/>  
    <CheckBox  
        android:id="@+id/cb02"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Check02"/>  
    <CheckBox  
        android:id="@+id/cb03"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Check03"/>  
    <CheckBox  
        android:id="@+id/cb04"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Check04"/>  
  
    <TextView  
        android:id="@+id/tv01"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:textSize="20sp"  
        android:text="[ Checked Boxes ]"/>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch05_compoundbutton;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.os.Bundle;  
import android.view.View;  
import android.widget.CheckBox;  
import android.widget.TextView;  
  
import java.util.ArrayList;  
import java.util.Arrays;  
  
public class MainActivity extends AppCompatActivity {  
    TextView tv;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
  
        tv = findViewById(R.id.tv01);  
  
        CheckClickListener listener = new CheckClickListener();  
        for ( int k : new int[]{R.id.cb01, R.id.cb02, R.id.cb03, R.id.cb04} ){  
            CheckBox cb = findViewById(k);  
            cb.setOnClickListener(listener);  
        }    }  
    class CheckClickListener implements View.OnClickListener {  
        @Override  
        public void onClick(View view) {  
            ArrayList<CharSequence> list = new ArrayList<>();  
            for (int k : new int[]{R.id.cb01, R.id.cb02, R.id.cb03, R.id.cb04}) {  
                CheckBox cb = findViewById(k);  
                if ( cb.isChecked() ) {  
                    list.add( cb.getText() );  
                }            }  
            String str = "[ Checked Boxes ]\n" + String.join(", ", list);  
            tv.setText(str);  
        }    }}
```