#ImageView
Ch03_counter

#### 실행 화면
![[Ch03_counter.png]]
- \[증가\] 버튼을 클릭하면 주사위의 눈이 랜덤으로 변하며 \[TextView\]에 눈의 수가 표시된다.
- \[감소\] 버튼을 클릭하면 \[TextView\]의 값이 1씩 줄어든다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:layout_margin="10dp"  
    android:orientation="vertical">  
  
    <TextView  
        android:id="@+id/tvCounter"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="Counter"  
        android:textSize="30sp"/>  
  
    <LinearLayout  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:orientation="horizontal">  
  
        <Button  
            android:id="@+id/btnIncrease"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_margin="5dp"  
            android:text="증가"/>  
        <Button  
            android:id="@+id/btnDecrease"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_margin="5dp"  
            android:text="감소"/>  
  
    </LinearLayout>  
  
    <ImageView  
        android:id="@+id/ivDice"  
        android:layout_width="200dp"  
        android:layout_height="200dp"/>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch03_counter;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.graphics.drawable.Drawable;  
import android.os.Bundle;  
import android.view.View;  
import android.widget.Button;  
import android.widget.ImageView;  
import android.widget.TextView;  
  
import java.util.Timer;  
import java.util.TimerTask;  
  
public class MainActivity extends AppCompatActivity {  
    TextView tvCounter;  
    int counter = 0;  
    ImageView ivDice;  
    int[] aryDice = {R.drawable.dice1, R.drawable.dice2, R.drawable.dice3,  
            R.drawable.dice4, R.drawable.dice5, R.drawable.dice6};  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
  
        //Initialize  
        tvCounter = findViewById(R.id.tvCounter);  
        Button btnIncrease = findViewById(R.id.btnIncrease);  
        Button btnDecrease = findViewById(R.id.btnDecrease);  
        ivDice = findViewById(R.id.ivDice);  
  
        //On Click Listener  
        btnIncrease.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View view) {  
                counter++;  
                tvCounter.setText("" + counter);  
                int randInt = (int)(Math.random() * 6) + 1;  
                tvCounter.setText(randInt + "");  
                ivDice.setImageResource( aryDice[randInt-1] );  
            }        });        btnDecrease.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View view) {  
                counter--;  
                tvCounter.setText("" + counter);  
            }        });    } //onCreate()  
}
```