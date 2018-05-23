# 性能优化与测试
## 一、UI性能优化
1. 由于View会不断刷新、变化，所以应尽量减少不必要的onMeasure、onDraw调用。
2. 对于ListView、GridView等需要Adapter加载数据的控件，在getView方法中应尽量减少访问耗资源的资源，例如，大量的写入文件操作，访问网络等。否则这些控件会出           现不时的停顿现象。如果非要访问这些资源，应将这些操作放到线程中。


```
public View getView(int position, View convertView, ViewGroup parent) {
ViewHolder holder;
if (convertView == null) {
final LayoutInflater inflater = (LayoutInflater)
mContext.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
convertView = inflater.inflate(R.layout.list_item_icon_text, null);
holder = new ViewHolder();
holder.icon = (ImageView) convertView.findViewById(R.id.icon);
holder.text = (TextView) convertView.findViewById(R.id.text);
convertView.setTag(holder);
} else {
holder = (ViewHolder) convertView.getTag();
}
holder.icon.setImageResource(R.drawable.icon);
holder.text.setText(mData[position]);
return convertView;
}
```

在getView方法中应convertView参数，而不要一味地创建新的视图对象，并且可以使用convertView.setTag和convertView.getTag保
存和获取视图对象.

## 二、避免ANR
ANR（Application Not Responding）:有时程序会出现ANR现象，解决方法是将耗资源的操作（如下载文件、复杂算法等）放到其他线程中。

## 三、执行时间测试
```
long start = System.currentTimeMillis();
... ...
long end = System.currentTimeMillis();
long time = end – start;
```

## 四、内存消耗测试
```
// 获取系统内存总数
long total = Runtime.getRuntime().totalMemory();
// 获取剩余内存
long free = Runtime.getRuntime().freeMemory();
// 返回已使用的内容
long used = total - free;
```

## 五、性能分析共工具:traceview

Traceview 是android平台配备一个很好的性能分析的工具。它可以通过图形化的方式让我们了解我们要跟踪的程序的性能，并且能具体到method。



```
Debug.startMethodTracing("activity_trace");
// 执行test1方法
test1();
// 执行test2方法
test2();
// 停止监视方法
Debug.stopMethodTracing();
```
会在SD卡的根目录生成一个activity_trace.trace文件。

## 六、自动化测试工具:Monkey
Monkey 是Android中的一个命令行工具，可以运行在模拟器里或实际设备中。它向系统发送伪随机的用户事件流(如按键输入、触摸屏输入、手势输入等)，实现对正在开发的应用程序进行压力测试。Monkey测试是一种为了测试软件的稳定性、健壮性的快速有效的方法。

`monkey -p com.android.calculator2 -v 5000`    其中-p表示对象包 –v 表示事件数量,表示打开计算器进行5000个随机事件操作。