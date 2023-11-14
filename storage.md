---
layout: page
title: Saving Data in your App
permalink: /storage/
---

# Saving Data in your App

One of the problems that you will likely face is how to storage data in your app. In this section I will show how to do this using Shared Preferences.

## Shared Preferences

Shared preferences are a way to save data in your app in the form of a key value store. A key value store is a type of storage where you store and retrieve your data based on a unique key.


### Create your MainActivity.java file

Paste the following code in your `MainActivity.java` file.


~~~
package com.thebook.bottomnav;

import android.content.Context;
import android.content.SharedPreferences;
import android.os.Bundle;
import android.util.Log;

import com.google.android.material.bottomnavigation.BottomNavigationView;

import org.json.JSONException;
import org.json.JSONObject;
import org.json.JSONStringer;

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



### Create your HomeFragment.java file

Paste the following code in your `HomeFragment.java` file.


~~~
package com.thebook.bottomnav.ui.home;

import android.os.Bundle;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;
import android.widget.TextView;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;
import androidx.lifecycle.Observer;
import androidx.lifecycle.ViewModelProviders;
import androidx.navigation.Navigation;

import com.thebook.bottomnav.R;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.util.Random;

public class HomeFragment extends Fragment implements View.OnClickListener{

    private HomeViewModel homeViewModel;
    private ImageView imageView;

    public View onCreateView(@NonNull LayoutInflater inflater,
                             ViewGroup container, Bundle savedInstanceState) {

        homeViewModel =
                ViewModelProviders.of(this).get(HomeViewModel.class);
        final View root = inflater.inflate(R.layout.fragment_home, container, false);
        final TextView textView = root.findViewById(R.id.text_home);
        homeViewModel.getText().observe(getViewLifecycleOwner(), new Observer<String>() {
            @Override
            public void onChanged(@Nullable String s) {
                //build the images here
                textView.setText(s);
                String poster = "";
                int length = 0;
                int rand = 0;

                try {
                    JSONArray jsonArray = new JSONArray(s);
                    length = jsonArray.length();

                    for (int i=4; i<16; i++){
                        rand = new Random().nextInt(length - 1);
                        JSONObject jsonObject = jsonArray.getJSONObject(rand);
                        poster = jsonObject.getString("poster");

                        // get id for an image view
                        int id = getResources().getIdentifier("imageView" + i,"id",
                                getActivity().getPackageName());
                        ImageView imgView = root.findViewById(id);

                        //get id for an image
                        int imgID = getResources().getIdentifier(poster , "drawable" ,
                                getActivity().getPackageName()) ;
                        imgView.setImageResource(imgID);
                    }
                } catch (JSONException e) {
                    e.printStackTrace();
                }
                //Log.d("SharedPref", poster);

            }
        });

        //imageView = root.findViewById(R.id.imageView4);
        //imageView.setOnClickListener(this);

        //add onClickListener to scrolling images
        for (int i=4; i<16; i++){
            int id = getResources().getIdentifier("imageView" + i,"id", getActivity().getPackageName());
            ImageView imgView = root.findViewById(id);
            imgView.setOnClickListener(this);
        }
        return root;
    }

    @Override
    public void onClick(View view) {
        String resourceName;
        Bundle bundle = new Bundle();

        resourceName = view.getResources().getResourceName(view.getId()).split("/")[1];
        bundle.putString("id", resourceName);
        Navigation.findNavController(view).navigate(R.id.action_navigation_home_to_navigation_info, bundle);
    }
}

~~~


### Create your HomeViewModel.java file

Paste the following code in your `HomeViewModel.java` file.


~~~
package com.thebook.bottomnav.ui.home;

import android.app.Application;
import android.content.Context;
import android.content.SharedPreferences;
import android.util.Log;

import java.util.Map;

import androidx.lifecycle.AndroidViewModel;
import androidx.lifecycle.LiveData;
import androidx.lifecycle.MutableLiveData;
import androidx.lifecycle.ViewModel;

//You need to extend AndroidViewModel
public class HomeViewModel extends AndroidViewModel {

    private MutableLiveData<String> mText;
    private static final String PREFS_NAME = "movie";

    public HomeViewModel(Application application) {
        super(application);
        mText = new MutableLiveData<>();
        mText.setValue("This is home fragment");

        SharedPreferences prefs = getApplication().getSharedPreferences(PREFS_NAME, Context.MODE_PRIVATE);
        String movieStr = prefs.getString("movie1","");

        mText.setValue(movieStr);

    }

    public LiveData<String> getText() {
        return mText;
    }
}
~~~


### Create your Assets

The Create your Assets section in the _Simple Kotlin Application_ chapter will take you through the steps on how to create assets for your application.

### Create your Layout

Enter the following code in your `fragment_home.xml` file.


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


### Run your App

The _Running your Simple App_ chapter will walk you through the steps on how to run this example in Android Studio.