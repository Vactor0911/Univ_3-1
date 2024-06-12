#Button 
CustomButton

#### 실행 화면
![[CustomButton.png]]
- 이미지가 등록된 버튼을 클릭한 후 드래그하면 마우스의 방향으로 회전하며 회전한 각도에 따라 볼륨의 퍼센트가 출력된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center">

    <kr.ac.mokwon.custombutton.VolumeView
        android:id="@+id/volume"
        android:layout_width="wrap_content"
        android:layout_height="200dp"
        android:src="@drawable/volume_knob"/>

    <TextView
        android:id="@+id/tvVolume"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textAlignment="center"
        android:textSize="50sp"
        android:textStyle="bold"
        android:text="0%"/>

</LinearLayout>
```

#### VolumeView.java
```java
package kr.ac.mokwon.custombutton;

import android.content.Context;
import android.graphics.Canvas;
import android.util.AttributeSet;
import android.view.MotionEvent;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.appcompat.widget.AppCompatImageView;

public class VolumeView extends AppCompatImageView {
    private double angle;
    private VolumeListener listener;
    private static final double RADIAN_TO_DEGRE = 180 / Math.PI;
    private static final int MAX_ANGLE = 120;

    public interface VolumeListener {
        public void onChange(double angle);
    }

    public VolumeView(@NonNull Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        setImageResource(R.drawable.volume_knob);
    }

    public VolumeView(@NonNull Context context) {
        super(context);
        setImageResource(R.drawable.volume_knob);
    }

    public void setOnVolumeListener(VolumeListener listener) {
        this.listener = listener;
    }

    private double getAngle(float x, float y) {
        float fixedX = x - (float) getWidth() / 2;
        float fixedY = (float) getHeight() / 2 - y;
        double degree = Math.atan2(fixedX, fixedY) * RADIAN_TO_DEGRE;
        degree = clamp(degree, -MAX_ANGLE, MAX_ANGLE);
        return degree;
    }

    private double clamp(double d, double min, double max) {
        if (d > max) {
            return max;
        }
        else if (d < min) {
            return min;
        }
        return d;
    }

    public int getVolume() {
        return (int)( (float)(angle + MAX_ANGLE) / (float)(MAX_ANGLE * 2) * 100);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        angle = getAngle( event.getX(), event.getY() );
        invalidate();
        listener.onChange(angle);
        return true;
    }

    @Override
    protected void onDraw(Canvas canvas) {
        canvas.rotate(  (float)angle, (float) getWidth() / 2, (float) getHeight() / 2 );
        super.onDraw(canvas);
    }
}
```

#### MainActivity.java
```java
package kr.ac.mokwon.custombutton;

import android.os.Bundle;
import android.widget.TextView;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        VolumeView volume = findViewById(R.id.volume);
        final TextView tvVolume = findViewById(R.id.tvVolume);
        tvVolume.setText( volume.getVolume() + "%" );
        volume.setOnVolumeListener(new VolumeView.VolumeListener() {
            @Override
            public void onChange(double angle) {
                tvVolume.setText( volume.getVolume() + "%" );
            }
        });
    }
}
```