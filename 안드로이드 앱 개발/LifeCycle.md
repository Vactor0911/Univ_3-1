#LifeCycle
Ch06_LifeCycle
#### 실행 화면
![[Ch06_LifeCycle.png]]
- 각 버튼을 클릭하면 해당하는 기능의 새로운 화면이 표시된다

#### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:id="@+id/main"  
    android:orientation="vertical"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:layout_margin="10dp">  
  
    <Button  
        android:id="@+id/btnWebBrowser"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:text="Web Browser"/>  
    <Button  
        android:id="@+id/btnCall"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:text="Call"/>  
    <Button  
        android:id="@+id/btnMap"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:text="Map"/>  
    <Button  
        android:id="@+id/btnContact"  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content"  
        android:text="Contact"/>  
  
</LinearLayout>
```

#### MainActivity.java
```java
package kr.ac.mokwon.ch06_lifecycle;

import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.util.Log;
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
        setContentView(R.layout.activity_main);

        Button btnWebBrowser = findViewById(R.id.btnWebBrowser);
        Button btnCall = findViewById(R.id.btnCall);
        Button btnMap = findViewById(R.id.btnMap);
        Button btnContact = findViewById(R.id.btnContact);

        btnWebBrowser.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent( Intent.ACTION_VIEW, Uri.parse("https://www.google.com") );
                startActivity(intent);
            }
        });
        btnCall.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent( Intent.ACTION_DIAL, Uri.parse("tel:(+82)12345678") );
                startActivity(intent);
            }
        });
        btnMap.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent( Intent.ACTION_VIEW, Uri.parse("geo:37.30,127.2?z=10") );
                startActivity(intent);
            }
        });
        btnContact.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent( Intent.ACTION_VIEW, Uri.parse("content://contacts/people/") );
                startActivity(intent);
            }
        });
    }

    @Override
    protected void onStart() {
        super.onStart();
        Log.d("LifeCycle", "onStart");
    }

    @Override
    protected void onResume() {
        super.onResume();
        Log.d("LifeCycle", "onResume");
    }

    @Override
    protected void onPause() {
        super.onPause();
        Log.d("LifeCycle", "onPause");
    }

    @Override
    protected void onStop() {
        super.onStop();
        Log.d("LifeCycle", "onStop");
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        Log.d("LifeCycle", "onDestroy");
    }
}
```

#### AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>  
<manifest xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools">  
  
    <uses-feature  
        android:name="android.hardware.telephony"  
        android:required="false"/>  
    <uses-feature  
        android:name="android.hardware.camera"  
        android:required="false" />  
  
    <uses-permission android:name="android.permission.CALL_PHONE"/>  
    <uses-permission android:name="android.permission.READ_CONTACTS"/>  
    <uses-permission android:name="android.permission.INTERNET"/>  
    <uses-permission android:name="android.permission.CAMERA"/>  
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>  
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>  
  
    <application  
        android:allowBackup="true"  
        android:dataExtractionRules="@xml/data_extraction_rules"  
        android:fullBackupContent="@xml/backup_rules"  
        android:icon="@mipmap/ic_launcher"  
        android:label="@string/app_name"  
        android:roundIcon="@mipmap/ic_launcher_round"  
        android:supportsRtl="true"  
        android:theme="@style/Theme.Ch06_LifeCycle"  
        tools:targetApi="31">  
        <activity  
            android:name=".MainActivity"  
            android:exported="true">  
            <intent-filter>  
                <action android:name="android.intent.action.MAIN" />  
  
                <category android:name="android.intent.category.LAUNCHER" />  
            </intent-filter>  
        </activity>  
    </application>  
</manifest>
```