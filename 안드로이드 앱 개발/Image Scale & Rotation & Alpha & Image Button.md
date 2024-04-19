#ImageView #ImageButton
Ch03_ImageView

#### 실행 화면
![[Ch03_ImageView.png]]
- \[Scale Type\] 버튼을 클릭하면 이미지의 scale type가 변경된다.
- \[Rotation\] 버튼을 클릭하면 이미지가 45도 회전한다.
- \[Alpha\] 버튼을 클릭하면 이미지의 알파 값이 변경된다.
- \[확대\] 버튼을 클릭하면 이전 이미지로 변경된다.
- \[축소\] 버튼을 클릭하면 다음 이미지로 변경된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:app="http://schemas.android.com/apk/res-auto"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:layout_margin="10dp"  
    android:orientation="vertical">  
    <!--  
    xmlns:app="http://schemas.android.com/apk/res-auto"는 이미지 표현 때  
    사용되는 경우가 있음    -->  
  
    <ImageView  
        android:id="@+id/imageView"  
        android:layout_width="match_parent"  
        android:layout_height="200dp"  
        android:src="@drawable/helldivers2"/>  
  
    <LinearLayout  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:gravity="center"  
        android:orientation="horizontal">  
  
        <Button  
            android:id="@+id/btnScaleType"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_margin="5dp"  
            android:layout_weight="1"  
            android:text="scale type"/>  
  
        <Button  
            android:id="@+id/btnRotation"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_margin="5dp"  
            android:layout_weight="1"  
            android:text="rotation"/>  
  
        <Button  
            android:id="@+id/btnAlpha"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_margin="5dp"  
            android:layout_weight="1"  
            android:text="alpha"/>  
  
    </LinearLayout>  
  
    <TextView  
        android:id="@+id/tvScaleType"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"/>  
  
    <LinearLayout  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:orientation="horizontal">  
  
        <ImageButton  
            android:id="@+id/imgBtnPlus"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:src="@android:drawable/btn_plus"/>  
        <ImageButton  
            android:id="@+id/imgBtnMinus"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:src="@android:drawable/btn_minus"/>  
  
    </LinearLayout>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch03_imageview;  
  
import androidx.appcompat.app.AppCompatActivity;  
  
import android.os.Bundle;  
import android.view.View;  
import android.widget.Button;  
import android.widget.ImageButton;  
import android.widget.ImageView;  
import android.widget.TextView;  
  
public class MainActivity extends AppCompatActivity {  
    ImageView imageView;  
    int scaleTypeIdx;  
    TextView tvScaleType;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_main);  
  
        imageView = findViewById(R.id.imageView);  
        Button btnScaleType = findViewById(R.id.btnScaleType);  
        Button btnRotation = findViewById(R.id.btnRotation);  
        Button btnAlpha = findViewById(R.id.btnAlpha);  
        tvScaleType = findViewById(R.id.tvScaleType);  
        tvScaleType.setText( imageView.getScaleType().toString() );  
  
        btnScaleType.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View view) {  
                ImageView.ScaleType[] scaleTypes = {  
                        ImageView.ScaleType.CENTER, ImageView.ScaleType.CENTER_CROP,  
                        ImageView.ScaleType.CENTER_INSIDE, ImageView.ScaleType.FIT_CENTER,  
                        ImageView.ScaleType.FIT_XY  
                };  
                imageView.setScaleType(scaleTypes[scaleTypeIdx]);  
                tvScaleType.setText( imageView.getScaleType().toString() );  
                scaleTypeIdx++;  
                if (scaleTypeIdx >= scaleTypes.length) {  
                    scaleTypeIdx = 0;  
                }            }        });        btnRotation.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View view) {  
                float rotation = imageView.getRotation();  
                imageView.setRotation(rotation + 45);  
            }        });        btnAlpha.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View view) {  
                float alpha = imageView.getAlpha();  
                imageView.setAlpha( (alpha == 1 ? 0.5f : 1) );  
            }        });  
        ImageButton imgBtnPlus = findViewById(R.id.imgBtnPlus);  
        ImageButton imgBtnMinus = findViewById(R.id.imgBtnMinus);  
  
        imgBtnPlus.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View view) {  
                imageView.setImageResource(R.drawable.helldivers2);  
            }        });        imgBtnMinus.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View view) {  
                imageView.setImageResource(R.drawable.super_destroyer);  
            }        });    }}
```