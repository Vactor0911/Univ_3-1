#Dialog 
Ch07_AlertDialog

#### 실행 화면
![[Ch07_AlertDialog_1.png|200]]![[Ch07_AlertDialog_2.png|200]]![[Ch07_AlertDialog_3.png|200]]
- 메인 화면의 `PopUp` 버튼을 클릭하면 팝업 대화창이 출력된다.
- 클릭한 팝업 대화창의 버튼 텍스트가 메인 화면에 출력된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:orientation="vertical"
    android:gravity="center"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/tvText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        android:textAlignment="center"
        android:textSize="30sp"/>
    <Button
        android:id="@+id/btnPopup"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="PopUp"/>
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch07_alertdialog;

import android.content.DialogInterface;
import android.graphics.Color;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.LinearLayout;
import android.widget.TextView;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AlertDialog;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import com.google.android.material.dialog.MaterialAlertDialogBuilder;

public class MainActivity extends AppCompatActivity {
    private TextView tvText;
    private final CharSequence[] aryItem = {"Red", "Green", "Blue", "Cyan", "Magenta", "Yellow", "Black"};
    private final int[] aryColor = {Color.RED, Color.GREEN, Color.BLUE, Color.CYAN, Color.MAGENTA, Color.YELLOW, Color.BLACK};

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

        tvText = findViewById(R.id.tvText);
        Button btnPopup = findViewById(R.id.btnPopup);
        btnPopup.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity.this);
                builder.setTitle("Alert Dialog Example");
                builder.setMessage("Are you sure to proceed the progress?");
                builder.setPositiveButton("Yes", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {
                        showAlertDialogRadio();
                    }
                });
                builder.setNegativeButton("No", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {
                        tvText.setText("No");
                        tvText.setTextColor(Color.BLACK);
                    }
                });
                builder.setNeutralButton("Cancel", new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialogInterface, int i) {
                        tvText.setText("Cancel");
                        tvText.setTextColor(Color.BLACK);
                    }
                });
                AlertDialog dialog = builder.create();
                dialog.show();
            }
        });
    }

    private class MyDialogInterface implements DialogInterface.OnClickListener {
        @Override
        public void onClick(DialogInterface dialogInterface, int i) {
            tvText.setText(aryItem[i]);
            switch (i) {
                case 1: case 3: case 5: case 6:
                    tvText.setTextColor(Color.WHITE);
                default:
                    tvText.setTextColor(Color.BLACK);
            }
            LinearLayout main = findViewById(R.id.main);
            main.setBackgroundColor(aryColor[i]);
        }
    }

    private void showAlertDialogItems() {

        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setTitle("Alert Dialog Items Example");
        builder.setItems( aryItem, new MyDialogInterface() );
        builder.setPositiveButton("Close", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) { }
        });
        AlertDialog dialog = builder.create();
        dialog.show();
    }

    private void showAlertDialogRadio() {
        AlertDialog.Builder builder = new AlertDialog.Builder(this);
        builder.setTitle("Alert Dialog Items Example");
        builder.setSingleChoiceItems( aryItem, -1, new MyDialogInterface() );
        builder.setPositiveButton("Close", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) { }
        });
        AlertDialog dialog = builder.create();
        dialog.show();
    }
}
```