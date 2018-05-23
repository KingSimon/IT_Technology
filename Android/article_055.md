# 多点触控之MotionEvent.ACTION_MASK作用
ACTION_MASK在Android中是应用于多点触摸操作，字面上的意思大概是动作掩码的意思吧。

在onTouchEvent(MotionEvent event)中，使用switch (event.getAction())可以处理ACTION_DOWN和ACTION_UP事件；

使用switch (event.getAction() & MotionEvent.ACTION_MASK)就可以处理处理多点触摸的ACTION_POINTER_DOWN和ACTION_POINTER_UP事件。

ACTION_DOWN和ACTION_UP就是单点触摸屏幕，按下去和放开的操作；

ACTION_POINTER_DOWN和ACTION_POINTER_UP就是多点触摸屏幕，当有一只手指按下去的时候，另一只手指按下和放开的动作捕捉；

ACTION_MOVE就是手指在屏幕上移动的操作；