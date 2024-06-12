#View 
ImageDisplay

#### 실행 화면
![[ImageDisplay_1.png]]![[ImageDisplay_2.png]]
- 버튼을 클릭하면 이미지의 크기와 각도가 변경된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:gravity="center">

        <Button
            android:id="@+id/btnExpand"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Scale +" />
        <Button
            android:id="@+id/btnShrink"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Scale -" />
        <Button
            android:id="@+id/btnRotateRight"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Rotate +" />
        <Button
            android:id="@+id/btnRotateLeft"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Rotate -" />
    </LinearLayout>

    <kr.ac.mokwon.imagedisplay.MyView
        android:id="@+id/my_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.imagedisplay;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        MyView myView = findViewById(R.id.my_view);
        Button btnExpand = findViewById(R.id.btnExpand);
        Button btnShrink = findViewById(R.id.btnShrink);
        Button btnRotateRight = findViewById(R.id.btnRotateRight);
        Button btnRotateLeft = findViewById(R.id.btnRotateLeft);

        btnExpand.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                myView.expand();
            }
        });
        btnShrink.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                myView.shrink();
            }
        });
        btnRotateRight.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                myView.rotateRight();
            }
        });
        btnRotateLeft.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                myView.rotateLeft();
            }
        });

    }
}
```

#### MyView.java
```java
package kr.ac.mokwon.imagedisplay;

import android.content.Context;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Matrix;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.view.View;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;

public class MyView extends View {
    float sx = 1f;
    float sy = 1f;
    float angle = 180f;

    public MyView(Context context) {
        super(context);
        setBackgroundColor(Color.GRAY);
    }

    public MyView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        setBackgroundColor(Color.GRAY);
    }

    @Override
    protected void onDraw(@NonNull Canvas canvas) {
        Bitmap b = BitmapFactory.decodeResource(getResources(), R.drawable.hellbomb); //이미지 파일을 Bitmap 객체로 변환
        Bitmap scaledB = Bitmap.createScaledBitmap(b, 200, 200, true); //filter 속성은 안티 앨리어싱 속성에 대한 사용 여부임. (보정)

        Matrix m = new Matrix();
        m.preScale(0.5f, -0.5f);
        Bitmap matrixB = Bitmap.createBitmap(b, 0, 0, b.getWidth(), b.getHeight(), m, true);

        int centerX = canvas.getWidth() / 2;
        int centerY = canvas.getHeight() / 2;
        canvas.scale(sx, sy, centerX, centerY);
        canvas.rotate(angle, centerX, centerY);

        //canvas.drawBitmap(scaledB, 0, 0, null);
        canvas.drawBitmap(matrixB, centerX - matrixB.getWidth() / 2, centerY - matrixB.getHeight() / 2, null);
    }

    public void expand() {
        sx += 0.3f;
        sy += 0.3f;
        invalidate();
    }

    public void shrink() {
        sx -= 0.3f;
        sy -= 0.3f;
        invalidate();
    }

    public void rotateRight() {
        angle += 30;
        invalidate();
    }

    public void rotateLeft() {
        angle -= 30;
        invalidate();
    }

}
```