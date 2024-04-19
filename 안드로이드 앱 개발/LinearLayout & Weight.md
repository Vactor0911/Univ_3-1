#Weight #LinearLayout
Ch04_LinearLayout

#### 실행 화면
![[Ch04_LinearLayout.png]]
- \[1\], \[2\], \[3\] 버튼은 각각 1 : 2 : 3의 비율로 화면에 배치된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <Button  
        android:id="@+id/btn01"  
        android:layout_width="wrap_content"  
        android:layout_height="0dp"  
        android:layout_weight="1"  
        android:text="1"/>  
    <Button  
        android:id="@+id/btn02"  
        android:layout_width="wrap_content"  
        android:layout_height="0dp"  
        android:layout_weight="2"  
        android:text="2"/>  
    <Button  
        android:id="@+id/btn03"  
        android:layout_width="wrap_content"  
        android:layout_height="0dp"  
        android:layout_weight="3"  
        android:text="3"/>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch04_linearlayout;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.os.Bundle;  
  
public class MainActivity extends AppCompatActivity {  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
    }}
```