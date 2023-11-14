---
layout: page
title: Background Tasks
permalink: /services/
---

# Background Tasks

We can think of a service as a process that can run in the background even when the user is not interacting with your app.
Services do not have a user interface but they do have a lifecycle.

## Create Service class

1.	To create a service, navigate to the directory in which you want to create it. In this case it is the _bottomnav_ directory. 

2.	Navigate to File -\> New -\> Service -\> Service

3.	Enter your Class Name in the "Configure Component" screen. In my case I called the class `SimpleService.java`. Click *Finish*.

Here is a full listing of the `SimpleService.java` file.


~~~
package com.thebook.bottomnav;

import android.app.Service;
import android.content.Intent;
import android.os.Handler;
import android.os.HandlerThread;
import android.os.IBinder;
import android.os.Looper;
import android.os.Message;
import android.os.Process; //using this instead of java.lang.Process
import android.util.Log;
import android.widget.Toast;

import static android.widget.Toast.LENGTH_LONG;

public class SimpleService extends Service {
    private Looper serviceLooper;
    private ServiceHandler serviceHandler;


    private final class ServiceHandler extends Handler {
        public ServiceHandler(Looper looper){
            super(looper);
        }

        @Override
        public void handleMessage(Message msg){
            try{
                Thread.sleep(5000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            stopSelf(msg.arg1);
            Log.d("Service->","Service has started");
        }

    }

    @Override
    public void onCreate() {
        HandlerThread thread = new HandlerThread("ServiceStartArguments", Process.THREAD_PRIORITY_BACKGROUND);
        thread.start();

        serviceLooper = thread.getLooper();
        serviceHandler = new ServiceHandler(serviceLooper);
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        Toast.makeText(this, "Service has started", LENGTH_LONG).show();
        Log.d("Service->","Service has started");

        Message msg = serviceHandler.obtainMessage();
        msg.arg1 = startId;
        serviceHandler.sendMessage(msg);

        return START_STICKY;
    }

    public void onDestroy(){
        Toast.makeText(this, "Service has ended", LENGTH_LONG);
        Log.d("Service->","Service has ended");
    }

    @Override
    public IBinder onBind(Intent intent) {

        return null;
    }
}

~~~


## Add Service to Manifest file

Once you create the service, this line will be added in between the `<application>` tags in your _Manifest.xml_ file.

        <service
            android:name=".SimpleService"
            android:enabled="true"
            android:exported="true">
        </service>

## Activate Service in MainActivity.java

Insert the following line in the `onCreate` method of your `MainActivity.java` file.

~~~
//start snippet
        startService(new Intent(this, SimpleService.class));
        //end snippet
~~~

Here is a full listing of the `MainActivity.java` file.


~~~
package com.thebook.bottomnav;

import android.content.Context;
import android.content.Intent;
import android.content.SharedPreferences;
import android.os.Bundle;
import com.google.android.material.bottomnavigation.BottomNavigationView;
import org.json.JSONException;
import androidx.appcompat.app.AppCompatActivity;
import androidx.navigation.NavController;
import androidx.navigation.Navigation;
import androidx.navigation.ui.AppBarConfiguration;
import androidx.navigation.ui.NavigationUI;

public class MainActivity extends AppCompatActivity {
    private static final String PREFS_NAME = "movie";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        BottomNavigationView navView = findViewById(R.id.nav_view);
        // Passing each menu ID as a set of Ids because each
        // menu should be considered as top level destinations.
        AppBarConfiguration appBarConfiguration = new AppBarConfiguration.Builder(
                R.id.navigation_home, R.id.navigation_dashboard, R.id.navigation_notifications)
                .build();
        NavController navController = Navigation.findNavController(this, R.id.nav_host_fragment);
        NavigationUI.setupActionBarWithNavController(this, navController, appBarConfiguration);
        NavigationUI.setupWithNavController(navView, navController);

        //start snippet
        startService(new Intent(this, SimpleService.class));
        //end snippet

        try {
            saveToSharedPreferences(this);
        } catch (JSONException e) {
            e.printStackTrace();
        }
    }

    @Override
    public boolean onSupportNavigateUp() {
        NavController navController = Navigation.findNavController(this, R.id.nav_host_fragment);
        return navController.navigateUp();
    }


    //user defined method
    private void saveToSharedPreferences(Context context) throws JSONException {
        SharedPreferences prefs = context.getSharedPreferences(PREFS_NAME, Context.MODE_PRIVATE);
        SharedPreferences.Editor preferencesEditor = prefs.edit();

        String movieJSONArray = "[{" +
                "\"director_name\": \"James Cameron\"," +
                "\"actor_2_name\": \"Joel David Moore\"," +
                "\"genres\": \"Action|Adventure|Fantasy|Sci-Fi\"," +
                "\"actor_1_name\": \"CCH Pounder\"," +
                "\"movie_title\": \"Avatar\"," +
                "\"plot_keywords\": \"avatar|future|marine|native|paraplegic\"," +
                "\"title_year\": 2009," +
                "\"poster\": \"tt37\"" +
                "},"

                + "{" +
                "    \"director_name\": \"Gore Verbinski\"," +
                "        \"actor_2_name\": \"Orlando Bloom\"," +
                "    \"genres\": \"Action|Adventure|Fantasy\"," +
                "    \"actor_1_name\": \"Johnny Depp\"," +
                "    \"movie_title\": \"Pirates of the Caribbean: At World's End\"," +
                "    \"plot_keywords\": \"goddess|marriage ceremony|marriage proposal|pirate|singapore\"," +
                "        \"title_year\": 2007," +
                "    \"poster\": \"tt38\"" +
                "  },"

                + "{" +
                "    \"director_name\": \"Sam Mendes\"," +
                "        \"actor_2_name\": \"Rory Kinnear\"," +
                "    \"genres\": \"Action|Adventure|Thriller\"," +
                "    \"actor_1_name\": \"Christoph Waltz\"," +
                "    \"movie_title\": \"Spectre\"," +
                "    \"plot_keywords\": \"bomb|espionage|sequel|spy|terrorist\"," +
                "        \"title_year\": 2015, " +
                "    \"poster\": \"tt39\"" +
                "  },"

                +"{" +
                "    \"director_name\": \"Christopher Nolan\"," +
                "        \"actor_2_name\": \"Christian Bale\"," +
                "    \"genres\": \"Action|Thriller\"," +
                "    \"actor_1_name\": \"Tom Hardy\"," +
                "    \"movie_title\": \"The Dark Knight Rises\"," +
                "    \"plot_keywords\": \"deception|imprisonment|lawlessness|police officer|terrorist plot\"," +
                "        \"title_year\": 2012," +
                "    \"poster\": \"tt40\"" +
                "  },"

                + "{" +
                "    \"director_name\": \"Doug Walker\"," +
                "        \"actor_2_name\": \"Rob Walker\"," +
                "    \"genres\": \"Documentary\"," +
                "    \"actor_1_name\": \"Doug Walker\"," +
                "    \"movie_title\": \"Star Wars: Episode VII - The Force Awakens\"," +
                "    \"plot_keywords\": \"\"," +
                "        \"title_year\": \"\"," +
                "    \"poster\": \"tt41\"" +
                "  },"

                + "{" +
                "    \"director_name\": \"Andrew Stanton\"," +
                "        \"actor_2_name\": \"Samantha Morton\"," +
                "    \"genres\": \"Action|Adventure|Sci-Fi\"," +
                "    \"actor_1_name\": \"Daryl Sabara\"," +
                "    \"movie_title\": \"John Carter\"," +
                "    \"plot_keywords\": \"alien|american civil war|male nipple|mars|princess\"," +
                "        \"title_year\": 2012," +
                "    \"poster\": \"tt42\"" +
                "  }]";

        preferencesEditor.putString("movie1",movieJSONArray);
        preferencesEditor.apply();

    }

}

~~~



## Create your ui files

We are now going to create the files that are related to our user interface. Specifically we will be editing the following files;

* `HomeFragment.java`
* `HomeViewModel.java`
* `InfoFragment.java`
* `InfoViewModel.java`

## Create your Layouts

Paste the following code into `fragment_home.xml`


~~~
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:id="@+id/container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="#070707"
        android:orientation="vertical">

        <ImageView
            android:id="@+id/imageView2"
            android:layout_width="51dp"
            android:layout_height="51dp"
            android:layout_gravity="left"
            android:layout_marginLeft="20dp"
            app:srcCompat="@drawable/myflix_icon" />


        <ImageView
            android:id="@+id/imageView3"
            android:layout_width="match_parent"
            android:layout_height="139dp"
            app:srcCompat="@drawable/main" />

        <TextView
            android:id="@+id/textView3"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Favorites"
            android:textColor="#FFFFFF"
            android:textSize="24sp" />

        <HorizontalScrollView
            android:layout_width="match_parent"
            android:layout_height="wrap_content">

            <LinearLayout
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:orientation="horizontal">

                <ImageView
                    android:id="@+id/imageView4"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:paddingLeft="10dp"
                    app:srcCompat="@drawable/tt37" />

                <ImageView
                    android:id="@+id/imageView5"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:paddingLeft="10dp"
                    app:srcCompat="@drawable/tt38" />

                <ImageView
                    android:id="@+id/imageView6"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:paddingLeft="10dp"
                    app:srcCompat="@drawable/tt39" />

                <ImageView
                    android:id="@+id/imageView7"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:paddingLeft="10dp"
                    app:srcCompat="@drawable/tt40" />
            </LinearLayout>
        </HorizontalScrollView>

        <TextView
            android:id="@+id/textView4"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Drama"
            android:textColor="#FFFFFF"
            android:textSize="24sp" />

        <HorizontalScrollView
            android:layout_width="match_parent"
            android:layout_height="206dp">

            <LinearLayout
                android:layout_width="wrap_content"
                android:layout_height="match_parent"
                android:orientation="horizontal">

                <ImageView
                    android:id="@+id/imageView8"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:paddingLeft="10dp"
                    app:srcCompat="@drawable/tt42" />

                <ImageView
                    android:id="@+id/imageView9"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:paddingLeft="10dp"
                    app:srcCompat="@drawable/tt41" />

                <ImageView
                    android:id="@+id/imageView10"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:paddingLeft="10dp"
                    app:srcCompat="@drawable/tt40" />

                <ImageView
                    android:id="@+id/imageView11"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    android:paddingLeft="10dp"
                    app:srcCompat="@drawable/tt39" />
            </LinearLayout>
        </HorizontalScrollView>

        <TextView
            android:id="@+id/textView2"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Comedy"
            android:textColor="#FFFFFF"
            android:textSize="24sp" />

        <HorizontalScrollView
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <LinearLayout
                android:layout_width="wrap_content"
                android:layout_height="match_parent"
                android:orientation="horizontal">

                <ImageView
                    android:id="@+id/imageView12"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    app:srcCompat="@drawable/tt41" />

                <ImageView
                    android:id="@+id/imageView13"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    app:srcCompat="@drawable/tt39" />

                <ImageView
                    android:id="@+id/imageView14"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    app:srcCompat="@drawable/tt38" />

                <ImageView
                    android:id="@+id/imageView15"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_weight="1"
                    app:srcCompat="@drawable/tt42" />

            </LinearLayout>
        </HorizontalScrollView>

        <TextView
            android:id="@+id/text_gallery"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:textAlignment="center"
            android:textSize="20sp" />

        <TextView
            android:id="@+id/text_home"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginStart="8dp"
            android:layout_marginTop="8dp"
            android:layout_marginEnd="8dp"
            android:textAlignment="center"
            android:textSize="20sp"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintTop_toTopOf="parent" />

    </LinearLayout>
</ScrollView>
~~~


Paste the following code into `info_fragment.xml`


~~~
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ui.info.InfoFragment">

    <TextView
        android:id="@+id/text_info"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="Hello" />

</FrameLayout>
~~~


## Create your Assets

The Create your Assets section in the _Simple Kotlin Application_ chapter will
take you through the steps on how to create assets for your application.

## Run your App
The _Running your Simple App_ chapter will walk you through the steps on how
to run this example in Android Studio.


## Conclusion

Services are headless. The run in the background without a user interface. The create one we New Android Component wizard. Once it has been created, you should then call it with the `startService` method.