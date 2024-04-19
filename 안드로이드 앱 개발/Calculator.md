#Button
Week04_Question01

#### 실행 화면
![[Week04_Question01.png]]
- 사칙연산을 할 수 있는 계산기이다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_margin="10dp"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <TextView  
        android:id="@+id/tvResult"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:textSize="15sp"  
        android:textAlignment="textEnd"  
        android:padding="10dp"  
        android:textColor="#777777"/>  
    <TextView  
        android:id="@+id/tvInput"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:textSize="30sp"  
        android:textAlignment="textEnd"  
        android:padding="10dp"  
        android:textColor="#000000"  
        android:text="0"/>  
  
    <LinearLayout  
        android:orientation="horizontal"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_gravity="center">  
  
        <Button  
            android:id="@+id/btn7"  
            android:layout_width="0dp"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="7"  
            android:padding="0dp"  
            android:layout_margin="5dp"/>  
        <Button  
            android:id="@+id/btn8"  
            android:layout_width="0dp"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="8"  
            android:padding="0dp"  
            android:layout_margin="5dp"/>  
        <Button  
            android:id="@+id/btn9"  
            android:layout_width="0dp"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="9"  
            android:padding="0dp"  
            android:layout_margin="5dp"/>  
        <Button  
            android:id="@+id/btnSum"  
            android:layout_width="0dp"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="+"  
            android:padding="0dp"  
            android:layout_margin="5dp"/>  
    </LinearLayout>  
  
    <LinearLayout  
        android:orientation="horizontal"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_gravity="center">  
  
        <Button  
            android:id="@+id/btn4"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="4"  
            android:layout_margin="5dp"/>  
        <Button  
            android:id="@+id/btn5"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="5"  
            android:layout_margin="5dp"/>  
        <Button  
            android:id="@+id/btn6"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="6"  
            android:layout_margin="5dp"/>  
        <Button  
            android:id="@+id/btnSub"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="-"  
            android:layout_margin="5dp"/>  
    </LinearLayout>  
  
    <LinearLayout  
        android:orientation="horizontal"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_gravity="center">  
  
        <Button  
            android:id="@+id/btn1"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="1"  
            android:layout_margin="5dp"/>  
        <Button  
            android:id="@+id/btn2"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="2"  
            android:layout_margin="5dp"/>  
        <Button  
            android:id="@+id/btn3"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="3"  
            android:layout_margin="5dp"/>  
        <Button  
            android:id="@+id/btnDiv"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="÷"  
            android:layout_margin="5dp"/>  
    </LinearLayout>  
  
    <LinearLayout  
        android:orientation="horizontal"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_gravity="center">  
  
        <Button  
            android:id="@+id/btn0"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="0"  
            android:layout_margin="5dp"/>  
        <Button  
            android:id="@+id/btnClear"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="Clear"  
            android:textSize="10dp"  
            android:layout_margin="5dp"/>  
        <Button  
            android:id="@+id/btnResult"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="="  
            android:layout_margin="5dp"/>  
        <Button  
            android:id="@+id/btnMul"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_weight="1"  
            android:text="×"  
            android:layout_margin="5dp"/>  
    </LinearLayout>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.week04_question01;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.os.Bundle;  
import android.view.View;  
import android.widget.Button;  
import android.widget.TextView;  
  
public class MainActivity extends AppCompatActivity implements View.OnClickListener {  
    private TextView tvResult, tvInput;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
  
        tvResult = findViewById(R.id.tvResult);  
        tvInput = findViewById(R.id.tvInput);  
  
        int[] aryId = {R.id.btn0, R.id.btn1, R.id.btn2, R.id.btn3, R.id.btn4, R.id.btn5, R.id.btn6, R.id.btn7, R.id.btn8, R.id.btn9,  
        R.id.btnSum, R.id.btnSub, R.id.btnMul, R.id.btnDiv, R.id.btnResult, R.id.btnClear};  
        for (int k : aryId) {  
            Button btn = findViewById(k);  
            btn.setOnClickListener(this);  
        }    }  
    @Override  
    public void onClick(View view) {  
        switch ( view.getId() ) {  
            case R.id.btn0:  
                addNum(0);  
                break;  
            case R.id.btn1:  
                addNum(1);  
                break;  
            case R.id.btn2:  
                addNum(2);  
                break;  
            case R.id.btn3:  
                addNum(3);  
                break;  
            case R.id.btn4:  
                addNum(4);  
                break;  
            case R.id.btn5:  
                addNum(5);  
                break;  
            case R.id.btn6:  
                addNum(6);  
                break;  
            case R.id.btn7:  
                addNum(7);  
                break;  
            case R.id.btn8:  
                addNum(8);  
                break;  
            case R.id.btn9:  
                addNum(9);  
                break;  
            case R.id.btnSum:  
                addOperator('+');  
                break;  
            case R.id.btnSub:  
                addOperator('-');  
                break;  
            case R.id.btnMul:  
                addOperator('×');  
                break;  
            case R.id.btnDiv:  
                addOperator('÷');  
                break;  
            case R.id.btnResult:  
                calculate();  
                break;  
            case R.id.btnClear:  
                tvResult.setText("");  
                tvInput.setText("0");  
                break;  
        }    }  
    private void addNum(int number) {  
        String _text = (String) tvInput.getText();  
        int _num = Integer.parseInt(_text);  
        _num = _num * 10 + number;  
        tvInput.setText( Integer.toString(_num) );  
    }  
    private void addOperator(char operator) {  
        String _text = (String)tvResult.getText();  
  
        if ( _text.isEmpty() || _text.split(" ").length > 1 ){  
            _text = (String)tvInput.getText() + " " + operator;  
        }        else {  
            _text = (String)( (String) tvResult.getText() ).split(" ")[0] + " " + operator;  
        }  
        tvInput.setText("0");  
        tvResult.setText(_text);  
    }  
    private void calculate() {  
        String _text = (String) tvResult.getText();  
        String[] _aryData = _text.split(" ");  
        int _number = Integer.parseInt(_aryData[0]);  
        String operator = _aryData[1];  
  
        int result = _number;  
        int _number2 = Integer.parseInt( (String) tvInput.getText() );  
        switch (operator) {  
            case "+":  
                result += _number2;  
                break;  
            case "-":  
                result -= _number2;  
                break;  
            case "×":  
                result *= _number2;  
                break;  
            case "÷":  
                if (_number2 == 0) {  
                    return;  
                }                result /= _number2;  
                break;  
        }  
        tvResult.setText(_number + " " + operator + " " + _number2 + " =");  
        tvInput.setText( Integer.toString(result) );  
    }}
```