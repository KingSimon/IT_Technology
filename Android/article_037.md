# Android 多线程：使用Thread和Handler (从网络上获取图片)
当一个程序第一次启动时，Android会同时启动一个对应的主线程(Main Thread)，主线程主要负责处理与UI相关的事件，如：用户的按键事件，用户接触屏幕的事件以及屏幕绘图事件，并把相关的事件分发到对应的组件进行处理。所以主线程通常又被叫做UI线程。

比如说从网上获取一个图片，在一个ImageView中将其显示出来，这种涉及到网络操作的程序一般都是需要开一个线程完成网络访问，但是在获得图片后，是不能直接在网络操作线程中调用ImageView的相关方法的，因为其他线程中是不能直接访问主UI线程成员 。

完整代码：

注意的是：Manifest.xml文件中，要声明网络权限。
```
package com.xsjayz.thread;

import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.DefaultHttpClient;

import android.app.Activity;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.Toast;

/**
 * 使用Thread+Handler的方式实现在非UI线程发送消息通知UI线程更新界面
 *
 * <a href="http://my.oschina.net/arthor" class="referer" target="_blank">@author</a>  XuShaoJie
 * @version 2012-08-31
 */
public class ThreadHandlerActivity extends Activity {

    private static final int MSG_SUCCESS = 0;// 获取图片成功的标识
    private static final int MSG_FAILURE = 1;// 获取图片失败的标识

    private ImageView mImageView;
    private Button mButton;
    private Thread mThread;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main);

        mImageView = (ImageView) findViewById(R.id.imageView);
        mButton = (Button) findViewById(R.id.button);

        mButton.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View v) {
                // 如果线程还没启动，则启动新的线程
                if (mThread == null) {
                    mThread = new Thread(runnable);
                    mThread.start();

                    // 否则提示："线程已经启动"
                } else {
                    Toast.makeText(
                            getApplication(),
                            getApplication().getString(R.string.thread_started),
                            Toast.LENGTH_LONG).show();
                }
            }
        });
    }

    private Handler mHandler = new Handler() {
        // 重写handleMessage()方法，此方法在UI线程运行
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
            // 如果成功，则显示从网络获取到的图片
            case MSG_SUCCESS:
                mImageView.setImageBitmap((Bitmap) msg.obj);
                Toast.makeText(getApplication(),
                        getApplication().getString(R.string.get_pic_success),
                        Toast.LENGTH_LONG).show();
                break;
            // 否则提示失败
            case MSG_FAILURE:
                Toast.makeText(getApplication(),
                        getApplication().getString(R.string.get_pic_failure),
                        Toast.LENGTH_LONG).show();
                break;
            }
        }
    };

    Runnable runnable = new Runnable() {
        // 重写run()方法，此方法在新的线程中运行
        @Override
        public void run() {
            HttpClient httpClient = new DefaultHttpClient();
            // 从网络上获取图片
            HttpGet httpGet = new HttpGet(
                    "http://www.oschina.net/img/logo.gif");
            final Bitmap bitmap;
            try {
                HttpResponse httpResponse = httpClient.execute(httpGet);
                // 解析为图片
                bitmap = BitmapFactory.decodeStream(httpResponse.getEntity()
                        .getContent());
            } catch (Exception e) {
                mHandler.obtainMessage(MSG_FAILURE).sendToTarget();// 获取图片失败
                return;
            }

            // 获取图片成功，向UI线程发送MSG_SUCCESS标识和bitmap对象
            mHandler.obtainMessage(MSG_SUCCESS, bitmap).sendToTarget();
        }
    };

}
```