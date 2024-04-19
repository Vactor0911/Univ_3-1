#Button #Padding #Color
Ch03_02

#### 실행 화면
![[Ch03_02.png]]

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <TextView  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="RED"  
        android:layout_margin="5dp"  
        android:paddingTop="5dp"  
        android:paddingBottom="5dp"  
        android:paddingLeft="20dp"  
        android:paddingRight="20dp"  
        android:textAlignment="center"  
        android:textSize="30dp"  
        android:textColor="#FFFFFF"  
        android:background="#FF0000"/>  
  
    <TextView  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="GREEN"  
        android:layout_margin="5dp"  
        android:paddingTop="5dp"  
        android:paddingBottom="5dp"  
        android:paddingLeft="20dp"  
        android:paddingRight="20dp"  
        android:textAlignment="center"  
        android:textSize="30dp"  
        android:textColor="#FFFFFF"  
        android:background="#00FF00"/>  
  
    <TextView  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:text="BLUE"  
        android:layout_margin="5dp"  
        android:paddingTop="5dp"  
        android:paddingBottom="5dp"  
        android:paddingLeft="20dp"  
        android:paddingRight="20dp"  
        android:textAlignment="center"  
        android:textSize="30dp"  
        android:textColor="#FFFFFF"  
        android:background="#0000FF"/>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch03_02;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.os.Bundle;  
  
public class MainActivity extends AppCompatActivity {  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
    }}
```