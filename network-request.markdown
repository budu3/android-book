---
layout: page
title: Network Request
permalink: /network-request/
---

# Network Request

In this section we will learn how to make a request over the internet in your app.

## Internet Permission

Before we can connect our app to the internet we will need to give it permission to do so. We do this by adding an entry to the _AndroidManifest.xml_.


The following tag need to be added between the `<manfest></manifest>` tag.

	<uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>

## HttpURLConnect

We are going to use a couple of classes from the `java.net` package to connect to the internet.


~~~
//start snippet
        try {
            url = new URL("https://raw.githubusercontent.com/budu3/the-book/master/code/myflix/movies.json");

            HttpURLConnection conn = (HttpURLConnection)url.openConnection();

            Log.d("Service->","connected");
            br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            String output;
            StringBuilder builder = new StringBuilder();
            while ((output = br.readLine()) != null){
                builder.append(output);
            }
            br.close();
            Log.d("Worker->", builder.toString());
            result = builder.toString();
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        //end snippet
~~~

### Create your MainActivity.java file

Paste the following code in your `MainActivity.java` file.


~~~
package com.thebook.bottomnav;

import android.content.Context;
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
    //private static final String PREFS_NAME = "movie";
    private boolean bounded = false;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //start snippet
        BottomNavigationView navView = findViewById(R.id.nav_view);
        AppBarConfiguration appBarConfiguration = new AppBarConfiguration.Builder(
                R.id.navigation_home, R.id.navigation_dashboard, R.id.navigation_notifications)
                .build();
        NavController navController = Navigation.findNavController(this, R.id.nav_host_fragment);
        NavigationUI.setupActionBarWithNavController(this, navController, appBarConfiguration);
        NavigationUI.setupWithNavController(navView, navController);
        //end snippet
    }

    protected void onStart() {
        //String remoteData = null;
        super.onStart();

    }

    @Override
    protected void onResume() {
        super.onResume();
    }

    protected void onStop() {
        super.onStop();
    }

    @Override
    public boolean onSupportNavigateUp() {
        NavController navController = Navigation.findNavController(this, R.id.nav_host_fragment);
        return navController.navigateUp();
    }

    //user defined method
    /*
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
     */
}

~~~


### Create your UI files

Paste the following code in your `HomeFragment.java` file.


~~~
package com.thebook.bottomnav.ui.home;

import android.content.Context;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;
import android.widget.TextView;
import android.widget.Toast;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.fragment.app.Fragment;
import androidx.lifecycle.Observer;
import androidx.lifecycle.ViewModelProviders;
import androidx.navigation.Navigation;
import androidx.recyclerview.widget.LinearLayoutManager;
import androidx.recyclerview.widget.RecyclerView;

import com.thebook.bottomnav.R;

import java.util.ArrayList;

public class HomeFragment extends Fragment implements RecyclerViewAdapter.ItemClickListener{

    private HomeViewModel homeViewModel;
    private ImageView imageView;
    private  RecyclerView recyclerView;
    private RecyclerView recyclerViewDrama;
    private RecyclerViewAdapter adapter;
    private RecyclerViewAdapter adapterDrama;
    private HomeFragment home;
    public View onCreateView(@NonNull LayoutInflater inflater,
                             ViewGroup container, Bundle savedInstanceState) {

        ArrayList<SimpleViewModel> movieList = new ArrayList<>();

        homeViewModel =
                ViewModelProviders.of(this).get(HomeViewModel.class);
        final View root = inflater.inflate(R.layout.fragment_home, container, false);
        final TextView textView = root.findViewById(R.id.text_home);
        home = this;

        homeViewModel.getArrayList().observe(getViewLifecycleOwner(), new Observer<ArrayList<SimpleViewModel>>() {
            @Override
            public void onChanged(@Nullable ArrayList<SimpleViewModel> movieList) {
                // data to populate the RecyclerView with

                Context context = getContext();
                recyclerView = root.findViewById(R.id.fav_recyclerview);
                recyclerView.setHasFixedSize(true);
                recyclerView.setLayoutManager(new LinearLayoutManager(context, LinearLayoutManager.HORIZONTAL, false));
                adapter = new RecyclerViewAdapter(context, movieList);
                adapter.setClickListener(home);
                recyclerView.setAdapter(adapter);

                recyclerView = root.findViewById(R.id.drama_recyclerview);
                recyclerView.setLayoutManager(new LinearLayoutManager(context, LinearLayoutManager.HORIZONTAL, false));
                adapter = new RecyclerViewAdapter(context, movieList);
                adapter.setClickListener(home);
                recyclerView.setAdapter(adapter);
                recyclerView.scrollToPosition(2);

                recyclerView = root.findViewById(R.id.comedy_recyclerview);
                recyclerView.setLayoutManager(new LinearLayoutManager(context, LinearLayoutManager.HORIZONTAL, false));
                adapter = new RecyclerViewAdapter(context, movieList);
                adapter.setClickListener(home);
                recyclerView.setAdapter(adapter);
                recyclerView.scrollToPosition(3);
            }
        });
        //End of viewModel

        return root;
    }

    //@Override
    //todo: pass movielist to the bundle here
    public void onClick(View view) {
        String resourceName;
        Bundle bundle = new Bundle();

        resourceName = view.getResources().getResourceName(view.getId()).split("/")[1];
        bundle.putString("id", resourceName);
        Navigation.findNavController(view).navigate(R.id.action_navigation_home_to_navigation_info, bundle);
    }

    @Override
    public void onItemClick(View view, int position) {
        Bundle bundle = new Bundle();
        SimpleViewModel item = adapter.getItem(position);
        int image = item.getImage();
        String poster = item.getPoster();
        String title = item.getTitle();

        bundle.putInt("image", image);
        bundle.putString("poster", poster);
        bundle.putString("title", title);

        //Toast.makeText(getContext(), "You clicked " + adapter.getItem(position) + " on row number " + position, Toast.LENGTH_SHORT).show();
        Toast.makeText(getContext(), "You clicked  on position number " + position, Toast.LENGTH_SHORT).show();
        Navigation.findNavController(view).navigate(R.id.action_navigation_home_to_navigation_info, bundle);
    }
}

~~~


Paste the following code in your `HomeViewModel.java` file.


~~~
package com.thebook.bottomnav.ui.home;

import android.app.Application;
import android.content.Context;
import android.content.SharedPreferences;
import android.util.Log;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.util.ArrayList;

import androidx.lifecycle.AndroidViewModel;
import androidx.lifecycle.LiveData;
import androidx.lifecycle.MutableLiveData;
import androidx.lifecycle.Observer;
import androidx.work.OneTimeWorkRequest;
import androidx.work.WorkInfo;
import androidx.work.WorkManager;

//You need to extend AndroidViewModel
public class HomeViewModel extends AndroidViewModel {

    private MutableLiveData<String> mText;
    private MutableLiveData<ArrayList<SimpleViewModel>> movieLiveData;
    private ArrayList<SimpleViewModel> movieList;
    private static final String PREFS_NAME = "movie";

    public HomeViewModel(Application application) {
        super(application);
        movieLiveData = new MutableLiveData<>();
        movieList = new ArrayList<>();
        mText = new MutableLiveData<>();
        mText.setValue("This is home fragment");

        SharedPreferences prefs = getApplication().getSharedPreferences(PREFS_NAME, Context.MODE_PRIVATE);
        String movieStr = prefs.getString("movie1","");

        final OneTimeWorkRequest workRequest = new OneTimeWorkRequest.Builder(MyWorker.class).build();
        final OneTimeWorkRequest imageRequest = new OneTimeWorkRequest.Builder(MyImageWorker.class).build();

        WorkManager.getInstance().beginWith(workRequest).then(imageRequest).enqueue();
        WorkManager.getInstance().getWorkInfoByIdLiveData(workRequest.getId()).observeForever(new Observer<WorkInfo>() {
            String output = "";
            @Override
            public void onChanged(WorkInfo workInfo) {
                Log.d("HomeFragment->", "" + workInfo.getState());
                if (workInfo.getState() == WorkInfo.State.SUCCEEDED) {
                    Log.d("HomeFragment->", "I got here");
                    output = workInfo.getOutputData().getString("RemoteData");
                    Log.d("HomeFragment->",output);

                    try {
                        JSONArray jsonArray = new JSONArray(output);
                        for (int i=0; i<jsonArray.length(); i++){
                            JSONObject jsonObject = jsonArray.getJSONObject(i);
                            String poster = jsonObject.getString("poster");
                            String title = jsonObject.getString("movie_title");
                            Log.d("HomeViewModel->", " poster " + poster);
                            SimpleViewModel svm = new SimpleViewModel();
                            svm.setTitle(title);
                            svm.setPoster(poster.replace("images/",""));
                            //svm.setImage(imgID);
                            movieList.add(svm);

                        }
                    } catch (JSONException e) {
                        e.printStackTrace();
                    }
                }
            }
        });
        /*
        try {
            JSONArray jsonArray = new JSONArray(movieStr);
            int len = jsonArray.length();
            //Log.d("len", String.valueOf(len));
            for (int i=0; i<len; i++){
                JSONObject jsonObject = jsonArray.getJSONObject(i);
                //Log.d("bottomnav:json", jsonObject.toString(2));
                String poster = jsonObject.getString("poster");
                String title = jsonObject.getString("movie_title");
                //Log.d("bottomnav:movie_title", title);
                int imgID = getApplication().getResources().getIdentifier(poster,"drawable",getApplication().getPackageName());
                SimpleViewModel svm = new SimpleViewModel();
                svm.setTitle(title);
                svm.setPoster(poster);
                svm.setImage(imgID);
                movieList.add(svm);
                //Log.d("bottomnav:id", "" + imgID);
                //Log.d("bottomnav:movielist", movieList.toString());
            }
        } catch (JSONException e) {
            e.printStackTrace();
        }
        */
        mText.setValue(movieStr);
        movieLiveData.setValue(movieList);

    }

    public LiveData<ArrayList<SimpleViewModel>> getArrayList() {
        return movieLiveData;
    }
}
~~~


Paste the following code in your `HomeViewModel.java` file.


~~~
package com.thebook.bottomnav.ui.home;

import android.app.Application;
import android.content.Context;
import android.content.SharedPreferences;
import android.util.Log;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import java.util.ArrayList;

import androidx.lifecycle.AndroidViewModel;
import androidx.lifecycle.LiveData;
import androidx.lifecycle.MutableLiveData;
import androidx.lifecycle.Observer;
import androidx.work.OneTimeWorkRequest;
import androidx.work.WorkInfo;
import androidx.work.WorkManager;

//You need to extend AndroidViewModel
public class HomeViewModel extends AndroidViewModel {

    private MutableLiveData<String> mText;
    private MutableLiveData<ArrayList<SimpleViewModel>> movieLiveData;
    private ArrayList<SimpleViewModel> movieList;
    private static final String PREFS_NAME = "movie";

    public HomeViewModel(Application application) {
        super(application);
        movieLiveData = new MutableLiveData<>();
        movieList = new ArrayList<>();
        mText = new MutableLiveData<>();
        mText.setValue("This is home fragment");

        SharedPreferences prefs = getApplication().getSharedPreferences(PREFS_NAME, Context.MODE_PRIVATE);
        String movieStr = prefs.getString("movie1","");

        final OneTimeWorkRequest workRequest = new OneTimeWorkRequest.Builder(MyWorker.class).build();
        final OneTimeWorkRequest imageRequest = new OneTimeWorkRequest.Builder(MyImageWorker.class).build();

        WorkManager.getInstance().beginWith(workRequest).then(imageRequest).enqueue();
        WorkManager.getInstance().getWorkInfoByIdLiveData(workRequest.getId()).observeForever(new Observer<WorkInfo>() {
            String output = "";
            @Override
            public void onChanged(WorkInfo workInfo) {
                Log.d("HomeFragment->", "" + workInfo.getState());
                if (workInfo.getState() == WorkInfo.State.SUCCEEDED) {
                    Log.d("HomeFragment->", "I got here");
                    output = workInfo.getOutputData().getString("RemoteData");
                    Log.d("HomeFragment->",output);

                    try {
                        JSONArray jsonArray = new JSONArray(output);
                        for (int i=0; i<jsonArray.length(); i++){
                            JSONObject jsonObject = jsonArray.getJSONObject(i);
                            String poster = jsonObject.getString("poster");
                            String title = jsonObject.getString("movie_title");
                            Log.d("HomeViewModel->", " poster " + poster);
                            SimpleViewModel svm = new SimpleViewModel();
                            svm.setTitle(title);
                            svm.setPoster(poster.replace("images/",""));
                            //svm.setImage(imgID);
                            movieList.add(svm);

                        }
                    } catch (JSONException e) {
                        e.printStackTrace();
                    }
                }
            }
        });
        /*
        try {
            JSONArray jsonArray = new JSONArray(movieStr);
            int len = jsonArray.length();
            //Log.d("len", String.valueOf(len));
            for (int i=0; i<len; i++){
                JSONObject jsonObject = jsonArray.getJSONObject(i);
                //Log.d("bottomnav:json", jsonObject.toString(2));
                String poster = jsonObject.getString("poster");
                String title = jsonObject.getString("movie_title");
                //Log.d("bottomnav:movie_title", title);
                int imgID = getApplication().getResources().getIdentifier(poster,"drawable",getApplication().getPackageName());
                SimpleViewModel svm = new SimpleViewModel();
                svm.setTitle(title);
                svm.setPoster(poster);
                svm.setImage(imgID);
                movieList.add(svm);
                //Log.d("bottomnav:id", "" + imgID);
                //Log.d("bottomnav:movielist", movieList.toString());
            }
        } catch (JSONException e) {
            e.printStackTrace();
        }
        */
        mText.setValue(movieStr);
        movieLiveData.setValue(movieList);

    }

    public LiveData<ArrayList<SimpleViewModel>> getArrayList() {
        return movieLiveData;
    }
}
~~~


Paste the following code in your `MyWorker.java` file.


~~~
package com.thebook.bottomnav.ui.home;


import android.content.Context;
import android.util.Log;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;

import androidx.annotation.NonNull;
import androidx.work.Data;
import androidx.work.Worker;
import androidx.work.WorkerParameters;

public class MyWorker extends Worker {

    public MyWorker (Context context, WorkerParameters workerParams){
        super(context, workerParams);
    }

    @NonNull
    @Override
    public Result doWork() {
        String remoteData;
        remoteData = getRemoteData();

        Data outputData = new Data.Builder()
                .putString("RemoteData", remoteData)
                .build();
        return Result.success(outputData);
    }

    private String getRemoteData(){
        Log.d("Worker->", "I got here");

        URL url = null;
        BufferedReader br = null;
        String result = null;
        //start snippet
        try {
            url = new URL("https://raw.githubusercontent.com/budu3/the-book/master/code/myflix/movies.json");

            HttpURLConnection conn = (HttpURLConnection)url.openConnection();

            Log.d("Service->","connected");
            br = new BufferedReader(new InputStreamReader(conn.getInputStream()));
            String output;
            StringBuilder builder = new StringBuilder();
            while ((output = br.readLine()) != null){
                builder.append(output);
            }
            br.close();
            Log.d("Worker->", builder.toString());
            result = builder.toString();
        } catch (MalformedURLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        //end snippet
        return result;
    }

}

~~~


Paste the following code in your `RecyclerViewAdapter.java` file.


~~~
package com.thebook.bottomnav.ui.home;

import android.content.Context;
import android.graphics.BitmapFactory;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;
import android.widget.TextView;

import com.thebook.bottomnav.R;

import java.util.ArrayList;

import androidx.recyclerview.widget.RecyclerView;

public class RecyclerViewAdapter extends RecyclerView.Adapter<RecyclerViewAdapter.ViewHolder> {

    private ArrayList<SimpleViewModel> data;
    private LayoutInflater layoutInflater;
    private ItemClickListener mClickListener;
    private Context context;

    RecyclerViewAdapter(Context context, ArrayList<SimpleViewModel> data) {
        this.layoutInflater = LayoutInflater.from(context);
        this.data = data;
        this.context = context;
    }

    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = layoutInflater.inflate(R.layout.recyclerview_item, parent, false);
        return new ViewHolder(view);
    }

    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
        //String pathToPicture = context.getCacheDir() + "/a-ghost-story.jpg";
        SimpleViewModel movie = data.get(position);
        String poster = movie.getPoster();
        String pathToPicture = context.getCacheDir() + "/" + poster;
        //holder.myImageView.setImageResource(movie.getImage());
        holder.myImageView.setImageBitmap(BitmapFactory.decodeFile(pathToPicture));
        Log.d("RecyclerView->",pathToPicture);
    }

    @Override
    public int getItemCount() {
        return data.size();
    }

    public class ViewHolder extends RecyclerView.ViewHolder implements View.OnClickListener {

        TextView myTextView;
        ImageView myImageView;

        ViewHolder(View itemView) {
            super(itemView);
            myImageView = itemView.findViewById(R.id.imageView);
            itemView.setOnClickListener(this);
        }

        @Override
        public void onClick(View view) {
            if (mClickListener != null) mClickListener.onItemClick(view, getAdapterPosition());
        }
    }

    SimpleViewModel getItem(int id) {
        return data.get(id);
    }

    void setClickListener(ItemClickListener itemClickListener) {
        this.mClickListener = itemClickListener;
    }

    public interface ItemClickListener {
        void onItemClick(View view, int position);
    }
}
~~~


Paste the following code in your `SimpleViewModel.java` file.


~~~
package com.thebook.bottomnav.ui.home;

public class SimpleViewModel {
    private String poster;
    private String title;

    public int getImage() {
        return image;
    }

    public void setImage(int image) {
        this.image = image;
    }

    private int image;

    public void setPoster(String poster) {
        this.poster = poster;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getPoster() {
        return poster;
    }

    public String getTitle() {
        return title;
    }
    /*
    public void ViewModel(String poster, String title){
            this.poster = poster;
            this.title = title;
    }
    */
    public void SimpleViewModel(String title, int image){
        this.image = image;
        this.title = title;
    }

}

~~~


Paste the following code in your `ViewHolder.java` file.


~~~
package com.thebook.bottomnav.ui.home;

import android.view.View;
import android.widget.ImageView;
import android.widget.TextView;
import com.thebook.bottomnav.R;

import androidx.recyclerview.widget.RecyclerView;

public class ViewHolder extends RecyclerView.ViewHolder{

    TextView myTextView;
    ImageView myImageView;

    ViewHolder(View itemView) {
        super(itemView);
        //myTextView = itemView.findViewById(R.id.tvMovieName);
        myImageView = itemView.findViewById(R.id.imageView);
    }


}

~~~




### Create your Layouts

Paste the following code in your `fragment_home.xml` file


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

        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/fav_recyclerview"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />


        <TextView
            android:id="@+id/textView4"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Drama"
            android:textColor="#FFFFFF"
            android:textSize="24sp" />

        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/drama_recyclerview"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

        <TextView
            android:id="@+id/textView2"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Comedy"
            android:textColor="#FFFFFF"
            android:textSize="24sp" />

        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/comedy_recyclerview"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />

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


Paste the following code in your `info_fragment.xml` file


~~~
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ui.info.InfoFragment">

    <ImageView
        android:id="@+id/info_image"
        android:layout_width="match_parent"
        android:layout_height="280dp"
        android:scaleType="fitCenter"
        android:src="@drawable/ic_home_black_24dp" />

    <TextView
        android:id="@+id/text_info"
        android:layout_width="match_parent"
        android:layout_height="fill_parent"
        android:gravity="center"

        android:text="Hello" />

</FrameLayout>
~~~


Paste the following code in your `recyclerview_item.xml` file


~~~
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="150dp"
    android:layout_height="match_parent"
    android:padding="1dp">

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="80dp"
        android:layout_height="wrap_content"
        android:layout_weight="1"
        android:paddingLeft="1dp"
        app:srcCompat="@drawable/tt38" />
</LinearLayout>

~~~


### Create your assets

The Create your Assets section in the _Simple Kotlin Application_ chapter will take you through the steps on how to create assets for your application.

### Run your App

The _Running your Simple App_ chapter will walk you through the steps on how to run this example in Android Studio.

## Conclusion

Network access are an import part a developing an app. The most important steps in doing this are add internet access to your _AndroidManifest.xml_ file and the retrieving data over the internet using the `java.net` library.