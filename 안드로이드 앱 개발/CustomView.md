CustomView

#### 실행 화면
![[CustomView.png]]
- 팩맨이 입을 벌리고 오므리며 애니메이션이 무한 재생된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <kr.ac.mokwon.customview.MyView
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.customview;

import android.os.Bundle;

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
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });
    }
}
```

#### MyView.java
```java
package kr.ac.mokwon.customview;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.RectF;
import android.util.AttributeSet;
import android.view.View;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;

public class MyView extends View {
    float startAngle, sweepAngle;
    float angle = -45f;
    float speed = 4f;

    public MyView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }

    @Override
    protected void onDraw(@NonNull Canvas canvas) {
        Paint p = new Paint();
        p.setColor(Color.YELLOW);
        p.setAntiAlias(true);
        canvas.drawColor(Color.BLACK);
        canvas.drawArc(new RectF(10, 10, 300, 300), startAngle, sweepAngle, true, p);
        p.setColor(Color.BLACK);
        canvas.drawCircle(180, 70, 26, p);

        angle += speed;
        if (angle > 45f) {
            angle -= 90f;
        }
        startAngle = Math.abs(angle) * 0.5f;
        sweepAngle = 360 - Math.abs(angle);

        invalidate();
    }
}

```