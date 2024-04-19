#ScrollView
Ch05_ScrollView

#### 실행 화면
![[Ch05_ScrollView.png]]
- 여러 개의 버튼이 스크롤 뷰를 통해 위 아래로 넘겨진다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent">  
  
    <ScrollView  
        android:layout_width="match_parent"  
        android:layout_height="match_parent">  
  
        <LinearLayout  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:orientation="vertical">  
  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
            <Button  
                android:layout_width="wrap_content"  
                android:layout_height="wrap_content"  
                android:text="Button"/>  
  
        </LinearLayout>  
  
    </ScrollView>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch05_scrollview;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.os.Bundle;  
  
public class MainActivity extends AppCompatActivity {  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
    }}
```