#ListView
Ch08_CustomListView

#### 실행 화면
![[CustomListView_1.png]]![[CustomListView_2.png]]
- 사진이 포함된 리스트 뷰가 표시된다.
- 아이템을 클릭하면 아이템의 제목이 Toast로 출력된다.

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

#### my_item.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:gravity="center_vertical">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:background="#BBBBBB"/>

    <LinearLayout
        android:orientation="vertical"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="5dp">

        <TextView
            android:id="@+id/tvName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="20sp"
            android:textStyle="bold"/>
        <TextView
            android:id="@+id/tvDescription"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="15sp"/>

    </LinearLayout>

</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch08_customlistview;

import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ListView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

public class MainActivity extends AppCompatActivity {
    private final Integer[] aryImage = {
            R.drawable.eagle_strafing_run,
            R.drawable.eagle_airstrike,
            R.drawable.eagle_110mm_rocket_pods,
            R.drawable.eagle_napalm_airstrike,
            R.drawable.eagle_smoke_strike,
            R.drawable.eagle_cluster_bomb,
            R.drawable.eagle_500kg_bomb,
    };

    private final String[] aryName = {
            "Eagle Strafing Run",
            "Eagle Airstrike",
            "Eagle 110mm Rocket Pods",
            "Eagle Napalm Airstrike",
            "Eagle Smoke Strike",
            "Eagle Cluster Bomb",
            "Eagle 500kg Bomb"
    };

    private final String[] aryDescription = {
            "Eagle Stratagem",
            "Eagle Stratagem",
            "Eagle Stratagem",
            "Eagle Stratagem",
            "Eagle Stratagem",
            "Eagle Stratagem",
            "Eagle Stratagem"
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

        MyAdapter adapter = new MyAdapter(MainActivity.this, aryName, aryDescription, aryImage);
        ListView listView = findViewById(R.id.listView);
        listView.setAdapter(adapter);
        listView.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> adapterView, View view, int i, long l) {
                Toast.makeText(MainActivity.this, aryName[i], Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

#### MyAdapter.java
```java
package kr.ac.mokwon.ch08_customlistview;

import android.app.Activity;
import android.content.Context;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.ImageView;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;

public class MyAdapter extends ArrayAdapter<String> {
    Activity activity;
    String[] aryName, aryDescription;
    Integer[] aryImage;

    public MyAdapter(Activity context, String[] names, String[] descriptions, Integer[] images) {
        super(context, R.layout.my_item, names);
        activity = context;
        aryName = names;
        aryDescription = descriptions;
        aryImage = images;
    }

    @NonNull
    @Override
    public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
        View rowView = activity.getLayoutInflater().inflate(R.layout.my_item, null, true);

        ImageView imageView = rowView.findViewById(R.id.imageView);
        TextView tvName = rowView.findViewById(R.id.tvName);
        TextView tvDescription = rowView.findViewById(R.id.tvDescription);

        imageView.setImageResource(aryImage[position]);
        tvName.setText(aryName[position]);
        tvDescription.setText(aryDescription[position]);

        return rowView;
    }
}

```