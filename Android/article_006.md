# Android MediaPlayer 简单综合应用------列出sdcard里所有.mp3文件,并且可以点击播放!
大家好,我们今天要利用Android  MediaPlayer



## Step 1:preparation work.



mksdcard 512M sdcard.img



create a new avd named AndroidSdcard







push songs into sdcard(before you push,you make sure your avd is running,else the operation of push will not work):



adb push `f:/music/1.mp3 /sdcard`



## Step 2: Layout UI desigen:



create two .xml files we called song_item.xml and songlist.xml the code are:

song_item.xml:
```
<?xml version="1.0" encoding="utf-8"?>
<TextView android:id="@+id/text1" xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
```


songlist.xml:

```
<?xml version="1.0" encoding="UTF-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:orientation="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent">

    <ListView android:id="@id/android:list"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent"
              android:layout_weight="1"
              android:drawSelectorOnTop="false"/>

    <TextView android:id="@id/android:empty"
              android:layout_width="fill_parent"
              android:layout_height="fill_parent"
              android:text="No songs found on SD Card."/>
</LinearLayout>
```


## Step 3: the core code MeusicDemo.java:


```
package com.android.test;

import java.io.File;
import java.io.FilenameFilter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import android.app.ListActivity;
import android.media.MediaPlayer;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.ArrayAdapter;
import android.widget.ListView;

class Mp3Filter implements FilenameFilter {
    public boolean accept(File dir, String name) {
        return (name.endsWith(".mp3"));
    }
}

public class MeusicDemo extends ListActivity {

 private static final String MEDIA_PATH = new String("/sdcard/");
 private List<String> songs = new ArrayList<String>();
 private MediaPlayer mp = new MediaPlayer();

 @Override
    public void onCreate(Bundle icicle) {
        try {
         super.onCreate(icicle);
         setContentView(R.layout.songlist);
         updateSongList();
        } catch (NullPointerException e) {
         Log.v(getString(R.string.app_name), e.getMessage());
        }
    }

    public void updateSongList() {
     File home = new File(MEDIA_PATH);
  if (home.listFiles( new Mp3Filter()).length > 0) {
      for (File file : home.listFiles( new Mp3Filter())) {
       songs.add(file.getName());
      }

      ArrayAdapter<String> songList = new ArrayAdapter<String>(this,R.layout.song_item,songs);
      setListAdapter(songList);
  }
    }

 @Override
    protected void onListItemClick(ListView l, View v, int position, long id) {
  try {

   mp.reset();
   mp.setDataSource(MEDIA_PATH + songs.get(position));
   mp.prepare();
   mp.start();
  } catch(IOException e) {
   Log.v(getString(R.string.app_name), e.getMessage());
  }
 }
}
```


## Step 4:run it.the result like this:







now u can enjoy  the music.lol~



if u wanna the source code please leave your email address, i will send u!