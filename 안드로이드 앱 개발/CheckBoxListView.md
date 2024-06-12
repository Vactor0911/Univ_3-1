#ListView 
Week12_Question01

#### 실행 화면
![[CheckBoxListView_1.png]]![[CheckBoxListView_2.png]]
- 고유한 이름을 가진 체크 박스 여러 개가 저장된 리스트 뷰가 표시된다.
- 체크 박스를 조작하면 해당 체크 박스의 이름과 체크 여부를 Toast로 출력한다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ListView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.week12_question01;

import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.CheckBox;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {
    String[] aryName = {
            "Item1", "Item2", "Item3", "Item4", "Item5", "Item6", "Item7"
    };
    String[] aryCbName = {
            "CheckBox1", "CheckBox2", "CheckBox3", "CheckBox4", "CheckBox5", "CheckBox6", "CheckBox7"
    };

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

        MyAdapter adapter = new MyAdapter(MainActivity.this, aryName, aryCbName);
        ListView listView = findViewById(R.id.listView);
        listView.setAdapter(adapter);
    }
}
```

#### my_item.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>
    <CheckBox
        android:id="@+id/checkBox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:checked="false"/>

</LinearLayout>
```

#### MyAdapter.java
```java
package kr.ac.mokwon.week12_question01;

import android.app.Activity;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.CheckBox;
import android.widget.TextView;
import android.widget.Toast;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;

public class MyAdapter extends ArrayAdapter<String> {
    Activity activity;
    String[] aryName, aryCbName;

    public MyAdapter(Activity context, String[] aryName, String[] aryCbName) {
        super(context, R.layout.my_item, aryName);
        activity = context;
        this.aryName = aryName;
        this.aryCbName = aryCbName;
    }

    @NonNull
    @Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        View rowView = activity.getLayoutInflater().inflate(R.layout.my_item, null, true);

        TextView textView = rowView.findViewById(R.id.textView);
        CheckBox checkBox = rowView.findViewById(R.id.checkBox);

        textView.setText(aryName[position]);
        checkBox.setText(aryCbName[position]);

        checkBox.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                CheckBox cb = view.findViewById(R.id.checkBox);
                String flagCheck = (cb.isChecked() ? "체크" : "체크 해제");
                Toast.makeText(activity, cb.getText() + " : " + flagCheck, Toast.LENGTH_SHORT).show();
            }
        });

        return rowView;
    }
}
```