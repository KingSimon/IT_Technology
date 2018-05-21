# Android应用程序优化总结
####　1. 代码优化的关键有一下几点：
* 少用浮点运算、文件、pipe、数据库访问
* 用高效的方式：StringBuffer代替大量临时String，SoundPool代替多个MediaPlayer，texture代替canvas， Log.d() 代替System.out.print()，避免invalidate()
* 重视onMeasure/onLayout/onDraw/onTouchEvent/getView等函数的效率

####　2. 游戏开发需要注意一下几点：
* 少用new()/enum/Iterator/HashMap/Arrays.sort()/Class.getXXX()…
* 多用private、final、局部变量，
* 2D善用draw_texture、3D善用VBO顶点缓冲
* 触屏事件时，暂停接受运动感应事件
* 用NDK实现关键代码

####　3. 不要求速度时，可用WebView和网页实现界面
####　4. 编译时执行代码优化