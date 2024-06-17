#SurfaceView 

#### 실행 화면
![[EndTermExamPractice.png]]
- 공이 화면을 돌아다니며 창의 모서리에 닿을 때마다 방향이 전환된다
- `추가` 버튼을 클릭하면 새로운 공 1개가 추가된다
- `삭제` 버튼을 클릭하면 공 1개가 삭제된다

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <kr.ac.mokwon.endtermexampractice2.MySurfaceView
        android:id="@+id/surfaceView"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="6" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:orientation="horizontal"
        android:gravity="center">

        <Button
            android:id="@+id/btnAdd"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:layout_margin="10dp"
            android:text="추가"
            android:textSize="14sp" />
        <Button
            android:id="@+id/btnRemove"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:layout_margin="10dp"
            android:text="삭제"
            android:textSize="14sp" />

    </LinearLayout>

</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.endtermexampractice2;

import android.os.Bundle;
import android.view.SurfaceView;
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

        MySurfaceView sv = findViewById(R.id.surfaceView);
        Button btnAdd = findViewById(R.id.btnAdd);
        Button btnRemove = findViewById(R.id.btnRemove);

        btnAdd.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                sv.addBall();
            }
        });
        btnRemove.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                sv.removeBall();
            }
        });
    }
}
```

#### Ball.java
```java
package kr.ac.mokwon.endtermexampractice2;

import android.app.Activity;
import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.view.View;

public class Ball {
    int x, y;
    int xSpeed, ySpeed;
    int radius;
    static int WIDTH = 320;
    static int HEIGHT = 320;
    Paint p;

    public Ball(View view) {
        WIDTH = view.getWidth();
        HEIGHT = view.getHeight();

        radius = 5 + (int)(Math.random() * 15f);
        x = radius + (int)(Math.random() * (WIDTH - radius * 2) );
        y = radius + (int)(Math.random() * (HEIGHT - radius * 2) );

        int speed = 1 + (int)(Math.random() * 9);
        xSpeed = speed;
        ySpeed = speed;

        p = new Paint();
        int red = (int)(Math.random() * 25.5) * 10;
        int green = (int)(Math.random() * 25.5) * 10;
        int blue = (int)(Math.random() * 25.5) * 10;
        p.setColor( Color.argb(255, red, green, blue) );
    }

    public void update(Canvas canvas) {
        if (x <= radius || x >= WIDTH - radius) {
            xSpeed = -xSpeed;
        }
        if (y <= radius || y >= HEIGHT - radius) {
            ySpeed = -ySpeed;
        }

        x += xSpeed;
        y += ySpeed;

        canvas.drawCircle(x, y, radius, p);
    }
}
```

#### MyThread.java
```java
package kr.ac.mokwon.endtermexampractice2;

import android.graphics.Canvas;
import android.graphics.Color;
import android.view.SurfaceHolder;

public class MyThread extends Thread {
    SurfaceHolder holder;
    Ball[] aryBall;
    boolean isRunning;

    public MyThread(SurfaceHolder holder, Ball[] aryBall) {
        this.holder = holder;
        this.aryBall = aryBall;
    }

    @Override
    public void run() {
        while (isRunning) {
            Canvas canvas = null;
            try {
                canvas = holder.lockCanvas();
                canvas.drawColor(Color.BLACK);
                synchronized (holder) {
                    try {
                        for (Ball ball : aryBall) {
                            ball.update(canvas);
                        }
                    }
                    catch (Exception e) { }
                }
            }
            finally {
                if (canvas != null) {
                    holder.unlockCanvasAndPost(canvas);
                }
            }
        }
    }

    public void setRunning(boolean b) {
        isRunning = b;
    }

    public void setAryBall(Ball[] aryBall) {
        this.aryBall = aryBall;
    }
}
```

#### MySurfaceView.java
```java
package kr.ac.mokwon.endtermexampractice2;

import android.content.Context;
import android.util.AttributeSet;
import android.util.Log;
import android.view.MotionEvent;
import android.view.SurfaceHolder;
import android.view.SurfaceView;
import android.view.View;

import androidx.annotation.NonNull;

import java.util.ArrayList;

public class MySurfaceView extends SurfaceView implements SurfaceHolder.Callback {
    MyThread myThread;
    ArrayList<Ball> listBall = new ArrayList<>();

    public MySurfaceView(Context context, AttributeSet attrs) {
        super(context, attrs);
        SurfaceHolder holder = getHolder();
        holder.addCallback(this);

        myThread = new MyThread(holder, null);
    }

    @Override
    public void surfaceCreated(@NonNull SurfaceHolder holder) {
        myThread.setRunning(true);
        myThread.start();
    }

    @Override
    public void surfaceChanged(@NonNull SurfaceHolder holder, int format, int width, int height) { }

    @Override
    public void surfaceDestroyed(@NonNull SurfaceHolder holder) {
        myThread.setRunning(false);

        boolean retry = true;
        while (retry) {
            try {
                myThread.join();
                retry = false;
            }
            catch (Exception e) { }
        }
    }

    public void addBall() {
        Ball newBall = new Ball(this);
        listBall.add(newBall);
        Log.d("Tag", "AddBall");

        Ball[] newAryBall = new Ball[listBall.size()];
        myThread.setAryBall( listBall.toArray(newAryBall) );
    }

    public void removeBall() {
        if ( listBall.isEmpty() ) {
            return;
        }

        listBall.remove(0);

        Ball[] newAryBall = new Ball[listBall.size()];
        myThread.setAryBall( listBall.toArray(newAryBall) );
    }
}
```