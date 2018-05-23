# Android 自动更新代码
```
import java.io.File;

import java.io.FileOutputStream;

import java.io.IOException;

import java.io.InputStream;

import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.DefaultHttpClient;

import android.app.AlertDialog;
import android.app.Dialog;
import android.app.ProgressDialog;
import android.content.DialogInterface;
import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.os.Environment;
import android.os.Handler;

public class Update extends BaseActivity {
    public ProgressDialog pBar;
    private Handler handler = new Handler();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.update);
        Dialog dialog = new AlertDialog.Builder(Update.this).setTitle("系统更新")
                .setMessage("发现新版本，请更新！")// 设置内容
                .setPositiveButton("确定",// 设置确定按钮
                        new DialogInterface.OnClickListener() {

                            @Override
                            public void onClick(DialogInterface dialog,
                                    int which) {
                                pBar = new ProgressDialog(Update.this);
                                pBar.setTitle("正在下载");
                                pBar.setMessage("请稍候...");
                                pBar
                                        .setProgressStyle(ProgressDialog.STYLE_SPINNER);
                                downFile("http://url:8765/OA.apk");

                            }

                        }).setNegativeButton("取消",
                        new DialogInterface.OnClickListener() {
                            public void onClick(DialogInterface dialog,
                                    int whichButton) {
                                // 点击"取消"按钮之后退出程序

                            }
                        }).create();// 创建
        // 显示对话框
        dialog.show();

    }

    void downFile(final String url) {
        pBar.show();
        new Thread() {
            public void run() {
                HttpClient client = new DefaultHttpClient();
                // params[0]代表连接的url
                HttpGet get = new HttpGet(url);
                HttpResponse response;
                try {
                    response = client.execute(get);
                    HttpEntity entity = response.getEntity();
                    long length = entity.getContentLength();
                    InputStream is = entity.getContent();
                    FileOutputStream fileOutputStream = null;
                    if (is != null) {

                        File file = new File(Environment
                                .getExternalStorageDirectory(), "OA.apk");
                        fileOutputStream = new FileOutputStream(file);

                        byte[] buf = new byte[1024];
                        int ch = -1;
                        int count = 0;
                        while ((ch = is.read(buf)) != -1) {
                            // baos.write(buf, 0, ch);
                            fileOutputStream.write(buf, 0, ch);
                            count += ch;
                            if (length > 0) {

                            }

                        }

                    }
                    fileOutputStream.flush();
                    if (fileOutputStream != null) {
                        fileOutputStream.close();
                    }
                    down();
                } catch (ClientProtocolException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }

        }.start();

    }

    void down() {
        handler.post(new Runnable() {
            public void run() {
                pBar.cancel();
                update();
            }
        });

    }

    void update() {

        Intent intent = new Intent(Intent.ACTION_VIEW);
        intent.setDataAndType(Uri.fromFile(new File("/sdcard/OA.apk")),
                "application/vnd.android.package-archive");
        startActivity(intent);
    }

}
```