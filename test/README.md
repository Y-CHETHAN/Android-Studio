# activity_main
```java
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/get_contacts"
        android:onClick="btnGetContactPressed"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:ignore="UsingOnClickInXml" />
</androidx.constraintlayout.widget.ConstraintLayout>
```
# MainActivity.java
```java
package com.example.contentprovider;

import androidx.appcompat.app.AppCompatActivity;
import androidx.core.app.ActivityCompat;
import androidx.core.content.ContextCompat;

import android.Manifest;
import android.annotation.SuppressLint;
import android.content.ContentResolver;
import android.content.pm.PackageManager;
import android.database.Cursor;
import android.net.Uri;
import android.os.Bundle;
import android.provider.ContactsContract;
import android.util.Log;
import android.view.View;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
    public void btnGetContactPressed(View v){
        getPhoneContacts();
    }
    private void getPhoneContacts(){
        if(ContextCompat.checkSelfPermission(this , Manifest.permission.READ_CONTACTS)
                != PackageManager.PERMISSION_GRANTED)
        {
            ActivityCompat.requestPermissions(this, new String[] {Manifest.permission.READ_CONTACTS}, 0) ;

        }
        ContentResolver contentResolver = getContentResolver();
        Uri uri= ContactsContract.CommonDataKinds.Phone.CONTENT_URI;
        @SuppressLint("Recycle") Cursor cursor = contentResolver.query(uri, null , null,null,null);
        Log.i("CONTACT_PROVIDER_DEMO","TOTAL # of Contacts ::: "+ cursor.getCount());
        if (cursor.getCount() > 0) {
            while(cursor.moveToNext()){
                @SuppressLint("Range") String contactName = cursor.getString(cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME));
                @SuppressLint("Range") String contactNumber = cursor.getString(cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER));

                Log.i("CONTACT_PROVIDER_DEMO","Contact Name :::  "+ contactName+"   PH #   :::"+ contactNumber);
            }
        }
    }

}

```
# Manifest
```java
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.contentprovider">

    <uses-permission android:name="android.permission.READ_CONTACTS"></uses-permission>
    <uses-permission android:name="android.permission.WRITE_CONTACTS"></uses-permission>

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Contentprovider">
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
```<img width="960" alt="img1" src="https://user-images.githubusercontent.com/75234991/163917737-0ff22e18-480a-4d0a-9483-5c20adb53400.png">

<img width="960" alt="img2" src="https://user-images.githubusercontent.com/75234991/163917741-a8b0bc06-5e5b-40f2-a788-05382315d221.png">
<img width="960" alt="img3" src="https://user-images.githubusercontent.com/75234991/163917752-ad144660-b1ce-44d9-8b8b-42850b8ee166.png">
<img width="944" alt="output" src="https://user-images.githubusercontent.com/75234991/163917761-078bc8f2-f6e1-4e5c-a364-370da383f42a.png">