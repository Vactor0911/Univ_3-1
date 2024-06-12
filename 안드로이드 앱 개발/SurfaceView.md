#SurfaceView
SurfaceView

#### 실행 화면
![[SurfaceView.png]]
- 랜덤 색상과 크기, 속도의 공이 화면에서 이리저리 튕기며 이동한다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!" />

</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.surfaceview;

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
        MySurfaceView sv = new MySurfaceView(this);
        setContentView(sv);
    }
}
```

#### Ball.java
```java
package kr.ac.mokwon.surfaceview;

import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;

public class Ball {
    int x, y;
    int xSpeed, ySpeed;
    int radius;
    static int WIDTH = 320;
    static int HEIGHT = 320;
    Paint p;

    public Ball(int radius) {
        this.radius = radius;
        x = radius + (int)( Math.random() * (WIDTH - radius * 2) );
        y = radius + (int)( Math.random() * (HEIGHT - radius * 2) );

        xSpeed = 1 + (int)(Math.random() * 9);
        ySpeed = 1 + (int)(Math.random() * 9);

        //Color
        p = new Paint();
        int red = (int)(Math.random() * 25.5) * 10;
        int green = (int)(Math.random() * 25.5) * 10;
        int blue = (int)(Math.random() * 25.5) * 10;
        p.setColor( Color.argb(255, red, green, blue) );
    }

    public void draw(Canvas canvas) {
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
package kr.ac.mokwon.surfaceview;

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
                    for (Ball ball : aryBall) {
                        ball.draw(canvas);
                    }
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
}
```

#### MySurfaceView.java
```java
package kr.ac.mokwon.surfaceview;

import android.content.Context;
import android.view.SurfaceHolder;
import android.view.SurfaceView;

import androidx.annotation.NonNull;

public class MySurfaceView extends SurfaceView implements SurfaceHolder.Callback { //Callback 함수는 SurfaceView가 정상적으로 생성되었는지 확인하기 위해 존재
    MyThread myThread;

    public MySurfaceView(Context context) {
        super(context);
        SurfaceHolder holder = getHolder();
        holder.addCallback(this);

        Ball[] aryBall = new Ball[10];
        for (int i=0; i<aryBall.length; i++) {
            int radius = 5 + (int)(Math.random() * 15f);
            aryBall[i] = new Ball(radius);
        }

        myThread = new MyThread(holder, aryBall);
    }

    @Override
    public void surfaceCreated(@NonNull SurfaceHolder surfaceHolder) {
        myThread.setRunning(true);
        myThread.start();
    }

    @Override
    public void surfaceChanged(@NonNull SurfaceHolder surfaceHolder, int i, int i1, int i2) {

    }

    @Override
    public void surfaceDestroyed(@NonNull SurfaceHolder surfaceHolder) {
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
}
```