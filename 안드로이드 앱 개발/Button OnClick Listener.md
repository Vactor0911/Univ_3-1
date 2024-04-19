#Button #OnClick
ButtonEvent_Week02
#### 실행 화면
![[ButtonEvent_Week02.png]]
- \[**THIRD BUTTON**\]을 누르면 \[**TextView**\]에 Third Button이 표시된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <Button  
        android:padding="20dp"  
        android:id="@+id/btn01"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="First Button"  
    />  
    <Button  
        android:layout_margin="10dp"  
        android:id="@+id/btn02"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Second Button"  
    />  
    <Button  
        android:id="@+id/btn03"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Third Button"  
        android:onClick="button_clicked"  
    />  
    <TextView  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:text="TextView"  
        android:id="@+id/tv01"  
    />  
  
    <!--match_parent를 사용하면 남은 공간을 모두 할당받아 크기가 지정된다.  
    wrap_content는 필요한 만큼만 할당받는다.  
    수치를 직접 입력하면 고정된 크기 값을 할당받는다. (단위 = dp)-->  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.buttonevent_week02;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.os.Bundle;  
import android.util.Log;  
import android.view.View;  
import android.widget.Button;  
import android.widget.TextView;  
  
public class MainActivity extends AppCompatActivity {  
    private TextView tv1;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
  
        Button btn1 = findViewById(R.id.btn01);  
        Button btn2 = findViewById(R.id.btn02);  
        Button btn3 = findViewById(R.id.btn03);  
        tv1 = findViewById(R.id.tv01);  
        ButtonListener listener = new ButtonListener(); //리스너 내부 클래스 선언  
        btn1.setOnClickListener(listener);  
        btn2.setOnClickListener(new View.OnClickListener(){ //클래스 직접 선언  
            @Override  
            public void onClick(View view){  
                Log.i("mokwon", ((Button)view).getText() + " Clicked!" );  
            }        });    }  
    public void button_clicked(View view) {  
        Button btn = (Button)view;  
        Log.i("mokwon", btn.getText() + " Called by Method!");  
        tv1.setText( btn.getText() );  
    }  
    public class ButtonListener implements View.OnClickListener {  
        @Override  
        public void onClick(View view) {  
            Log.i("mokwon", "button click >> " + ((Button)view).getText() );  
        }    }}
```