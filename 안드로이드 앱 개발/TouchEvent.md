#TouchEvent
TouchEvent

#### 실행 화면
![[TouchEvent.png]]
- 빈 공간을 드래그하면 파란색으로 그려진다.
- `Clear` 버튼을 클릭하면 그렸던 내용이 초기화된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:id="@+id/btnClear"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Clear"
        android:textSize="20sp"
        android:textStyle="bold"
        android:layout_margin="10dp"
        android:layout_gravity="center"/>

    <kr.ac.mokwon.touchevent.SingleView
        android:id="@+id/SingleView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.touchevent;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //MyView view = new MyView(this);
        setContentView(R.layout.activity_main);

        Button btnClear = findViewById(R.id.btnClear);
        btnClear.setOnClickListener(this);
    }

    @Override
    public void onClick(View view) {
        SingleView singleView = findViewById(R.id.SingleView);
        singleView.clear();
    }
}
```

#### SingleView.java
```java
package kr.ac.mokwon.touchevent;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.util.AttributeSet;
import android.util.Log;
import android.view.MotionEvent;
import android.view.View;
import android.graphics.Paint;
import android.graphics.Path;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;

public class SingleView extends View {
    Paint p = new Paint();
    Path path = new Path();
    static boolean flagClear = false;

    public SingleView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        p.setAntiAlias(true);
        p.setStrokeWidth(3);
        p.setColor(Color.BLUE);
        p.setStyle(Paint.Style.STROKE);
        p.setStrokeJoin(Paint.Join.ROUND);
    }

    public SingleView(Context context) {
        this(context, null);
    }

    public void clear() {
        path.reset();
        invalidate();
    }

    @Override
    protected void onDraw(@NonNull Canvas canvas) {
        canvas.drawPath(path, p);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        float x = event.getX();
        float y = event.getY();
        switch ( event.getAction() ) {
            case MotionEvent.ACTION_DOWN:
                path.moveTo(x, y);
                break;
            case MotionEvent.ACTION_MOVE:
                path.lineTo(x, y);
                break;
            case MotionEvent.ACTION_UP:
                break;
            default:
                return false;
        }
        invalidate();
        return true;
    }
}
```

#### MyView.java
```java
package kr.ac.mokwon.touchevent;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.view.MotionEvent;
import android.view.View;

import androidx.annotation.NonNull;
import androidx.constraintlayout.widget.ConstraintSet;

import java.util.ArrayList;
import java.util.Random;

public class MyView extends View {
    ArrayList<Circle> listCircle;
    Paint p;

    public MyView(Context context) {
        super(context);
        p = new Paint();
        listCircle = new ArrayList<>();
    }

    @Override
    protected void onDraw(@NonNull Canvas canvas) {
        super.onDraw(canvas);
        for (Circle circle : listCircle) {
            p.setColor(circle.color);
            canvas.drawCircle(circle.x, circle.y, circle.radius, p);
        }
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if (event.getAction() == MotionEvent.ACTION_DOWN) {
            Random random = new Random();
            float radius = random.nextInt(100);
            int color = Color.rgb(random.nextInt(256), random.nextInt(256), random.nextInt(256));
            Circle circle = new Circle(event.getX(), event.getY(), radius, color);
            listCircle.add(circle);
            invalidate();
            return true;
        }
        return super.onTouchEvent(event);
    }
}
```

#### Circle.java
```java
package kr.ac.mokwon.touchevent;

public class Circle {
    public float x, y, radius;
    public int color;

    public Circle(float x, float y, float radius, int color) {
        this.x = x;
        this.y = y;
        this.radius = radius;
        this.color = color;
    }
}
```