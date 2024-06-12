#ListView 
Ch08_RecyclerView

#### 실행 화면
![[RecyclerView_1.png]]![[RecyclerView_2.png]]
- `ListView`와 동일하지만, 아이템 객체가 재활용되는 `RecyclerView`이다.
- 아이템을 클릭하면 아이템의 제목과 설명이 Toast로 출력된다.

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

</LinearLayout>
```

#### item.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_margin="10dp">

    <TextView
        android:id="@+id/tvName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Item"
        android:textSize="20sp"
        android:textStyle="bold"/>
    <TextView
        android:id="@+id/tvDescription"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Description"
        android:textSize="20sp"/>

</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch08_recyclerview;

import android.os.Bundle;
import android.view.View;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

public class MainActivity extends AppCompatActivity {
    String[] aryName, aryDescription;
    static final int LENGTH = 20;

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

        //Initialize
        aryName = new String[LENGTH];
        aryDescription = new String[LENGTH];

        for (int i=0; i<LENGTH; i++) {
            aryName[i] = "Item " + (i+1);
            aryDescription[i] = "description " + (i+1);
        }

        RecyclerView recyclerView = findViewById(R.id.recyclerView);
        recyclerView.setLayoutManager( new LinearLayoutManager(this) );
        MyAdapter adapter = new MyAdapter(this, aryName, aryDescription);
        adapter.setItemClickListener(new MyAdapter.ItemClickListener() {
            @Override
            public void onItemClick(View view, int i) {
                String text = aryName[i] + " >> " + aryDescription[i];
                Toast.makeText(MainActivity.this, text, Toast.LENGTH_SHORT).show();
            }
        });
        recyclerView.setAdapter(adapter);
    }
}
```

#### MyAdapter.java
```java
package kr.ac.mokwon.ch08_recyclerview;

import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;

public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder> {
    String[] aryName, aryDescription;
    Context context;
    ItemClickListener listener;

    public  interface ItemClickListener {
        void onItemClick(View view, int i);
    }

    public MyAdapter(Context context, String[] aryName, String[] aryDescription) {
        this.context = context;
        this.aryName = aryName;
        this.aryDescription = aryDescription;
    }

    public void setItemClickListener(ItemClickListener listener) {
        this.listener = listener;
    }

    @NonNull
    @Override
    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(context).inflate(R.layout.item, parent, false);
        return new ViewHolder(view);
    }

    @Override
    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
        String strName = aryName[position];
        String strDescription = aryDescription[position];
        holder.setText(strName, strDescription);
    }

    @Override
    public int getItemCount() {
        return aryName.length;
    }

    public class ViewHolder extends RecyclerView.ViewHolder implements View.OnClickListener {
        TextView tvName, tvDescription;

        public ViewHolder(@NonNull View itemView) {
            super(itemView);
            tvName = itemView.findViewById(R.id.tvName);
            tvDescription = itemView.findViewById(R.id.tvDescription);
            itemView.setOnClickListener(this);
        }

        @Override
        public void onClick(View view) {
            if (listener != null) { //사용자가 리스너를 등록하지 않을 수도 있기 때문에 리스너의 null 체크는 필수임.
                listener.onItemClick( view, getAdapterPosition() );
            }
        }

        public void setText(String name, String description) {
            tvName.setText(name);
            tvDescription.setText(description);
        }
    } //ViewHolder 클래스
} //MyAdapter 클래스
```