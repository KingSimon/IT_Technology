# Android获取bitmap的方法
## 第一种方法

通过BitmapDrawable对象获得bitmap

```
//得到Resources对象
Resources r = this.getContext().getResources();
//以数据流的方式读取资源
Inputstream is = r.openRawResource(R.drawable.my_background_image);
BitmapDrawable bmpDraw = new BitmapDrawable(is);
Bitmap bmp = bmpDraw.getBitmap();
```
## 第二种方法

使用BitmapFactory

```
InputStream is = getResources().openRawResource(R.drawable.icon);
 Bitmap mBitmap = BitmapFactory.decodeStream(is);
```

## 第三种方法

```
((BitmapDrawable) context.getResources().getDrawable(id)).getBitmap()
```

其中第一、二种方法获取的bitmap对象的width、height保持原大小

第三种方法获取的bitmap对象的width、height为原始大小X机器density