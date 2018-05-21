# Android MediaPlayer播放mp3的实例
大家好我们今天研究的是Android中很重要也最为复杂的媒体播放器---MediaPlayer.

Android的MediaPlayer包含了Audio和video的播放功能，在Android的界面上，Music和Video两个应用程序都是调用MediaPlayer实现的。



  MediaPlayer在底层是基于OpenCore(PacketVideo)的库实现的，为了构建一个MediaPlayer程序，上层还包含了进程间通讯等内容，这种进程间通讯的基础是Android基本库中的Binder机制。



而我们今天的例子只是利用MediaPlayer来播放res/raw文件夹中一首非常动听的英文哥love fool.mp3.程序有三个ImageButton按钮,播放,停止,和暂停!三个按钮的功能我就不用多说.下面我将Step By Step教你如何完成本Demo的实现.



## Step 1 :新建一个Android工程,命名为MediaPlayerDemo.



## Step 2 :准备素材,在res下建一个raw文件夹,将foollove.mp3导入,将play.png,pause.png,及stop.png导入res/drawable文件夹下.



## Step 3: 设计UI布局,在main.xml里放入三个ImageButton(这里可以用AbsoluteLayout,或者RelativeLayout实现,我用后者).代码如下:


```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
  android:layout_width="fill_parent"
  android:layout_height="fill_parent"
  android:background="@drawable/white"
  xmlns:android="http://schemas.android.com/apk/res/android "
>
  <TextView
    android:id="@+id/myTextView1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/hello"
    android:layout_alignParentTop="true"
    android:layout_alignParentLeft="true"
  >
  </TextView>
  <ImageButton
    android:id="@+id/myButton1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/play"
    android:layout_below="@+id/myTextView1"
  >
  </ImageButton>
  <ImageButton
    android:id="@+id/myButton3"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/pause"
    android:layout_alignTop="@+id/myButton1"
    android:layout_toRightOf="@+id/myButton1"
  >
  </ImageButton>
  <ImageButton
    android:id="@+id/myButton2"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/stop"
    android:layout_alignTop="@+id/myButton1"
    android:layout_toRightOf="@+id/myButton3"
  >
  </ImageButton>
</RelativeLayout>
```


## Step 4 :主控制程序MediaPlayerDemo.java的实现,代码如下:


```
package com.android.test;

import android.app.Activity;
import android.media.MediaPlayer;
import android.os.Bundle;
import android.view.View;
import android.widget.ImageButton;
import android.widget.TextView;

public class MediaPlayerDemo extends Activity {

 private ImageButton mb1,mb2,mb3;
 private TextView tv;
 private MediaPlayer mp;
 //声明一个变量判断是否为暂停,默认为false
 private boolean isPaused = false;
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        //通过findViewById找到资源
        mb1 = (ImageButton)findViewById(R.id.myButton1);
        mb2 = (ImageButton)findViewById(R.id.myButton2);
        mb3 = (ImageButton)findViewById(R.id.myButton3);
        tv = (TextView)findViewById(R.id.myTextView1);

        //创建MediaPlayer对象,将raw文件夹下的lovefool.mp3
        mp = MediaPlayer.create(this,R.raw.lovefool);
        //增加播放音乐按钮的事件
        mb1.setOnClickListener(new ImageButton.OnClickListener(){
   @Override
   public void onClick(View v) {
    try {
     if(mp != null)
     {
      mp.stop();
     }
     mp.prepare();
     mp.start();
     tv.setText("音乐播放中...");
    } catch (Exception e) {
     tv.setText("播放发生异常...");
     e.printStackTrace();
    }
   }
        });

        mb2.setOnClickListener(new ImageButton.OnClickListener(){
   @Override
   public void onClick(View v) {
    try {
     if(mp !=null)
     {
      mp.stop();
      tv.setText("音乐停止播放...");
     }
    } catch (Exception e) {
     tv.setText("音乐停止发生异常...");
     e.printStackTrace();
    }

   }
        });

        mb3.setOnClickListener(new ImageButton.OnClickListener(){
   @Override
   public void onClick(View v) {
    try {
     if(mp !=null)
     {
      if(isPaused==false)
      {
       mp.pause();
       isPaused=true;
       tv.setText("停止播放!");
      }
      else if(isPaused==true)
      {
       mp.start();
       isPaused = false;
       tv.setText("开始播发!");
      }
     }
    } catch (Exception e) {
     tv.setText("发生异常...");
     e.printStackTrace();
    }

   }
        });

        /* 当MediaPlayer.OnCompletionLister会运行的Listener */
        mp.setOnCompletionListener(
          new MediaPlayer.OnCompletionListener()
        {
          // @Override
          /*覆盖文件播出完毕事件*/
          public void onCompletion(MediaPlayer arg0)
          {
            try
            {
              /*解除资源与MediaPlayer的赋值关系
               * 让资源可以为其它程序利用*/
              mp.release();
              /*改变TextView为播放结束*/
              tv.setText("音乐播发结束!");
            }
            catch (Exception e)
            {
              tv.setText(e.toString());
              e.printStackTrace();
            }
          }
        });

        /* 当MediaPlayer.OnErrorListener会运行的Listener */
        mp.setOnErrorListener(new MediaPlayer.OnErrorListener()
        {
          @Override
          /*覆盖错误处理事件*/
          public boolean onError(MediaPlayer arg0, int arg1, int arg2)
          {
            // TODO Auto-generated method stub
            try
            {
              /*发生错误时也解除资源与MediaPlayer的赋值*/
              mp.release();
              tv.setText("播放发生异常!");
            }
            catch (Exception e)
            {
              tv.setText(e.toString());
              e.printStackTrace();
            }
            return false;
          }
        });
      }

}
```


## Step 5: 运行效果如下,一首动听的love fool在播放...享受中...













扩散学习:



如果我们想播放手机卡里的音乐,或者URL下载流媒体来播放,示意程序如下:


```
MediaPlayer mp = new MediaPlayer();

mp.setDataSource(String URL/FILE_PATH);

mp.prepare();

mp.start();
```
以上程序主要是通过MediaPlayer.setDataSource() 的方法,将URL或文件路径以字符串的方式传入.使用setDataSource ()方法时,要注意以下三点:

1.构建完成的MediaPlayer 必须实现Null 对像的检查.

2.必须实现接收IllegalArgumentException 与IOException 等异常,在很多情况下,你所用的文件当下并不存在.

3.若使用URL 来播放在线媒体文件,该文件应该要能支持pragressive 下载.