# Mobile Flex视图跳转优化处理
视图navigator相当一个栈，如无需保存视图状态，建议用popAll()清除所有视图再跳转到相应的视图。如以下代码：

SmartHomeMobile.getInstance().navigator.popAll();

SmartHomeMobile.getInstance().navigator.pushView(RoomListView);

System.gc();

如需要保存一个主要视图，避免多次创建，利用上述方法跳转进入该视图后，直接用pushView()跳转到下个视图。接下来的跳转代码如下：

SmartHomeMobile.getInstance().navigator.popToFirstView();

SmartHomeMobile.getInstance().navigator.pushView(RoomListView);

System.gc();

主要原理是将该视图存储到栈的第一个视图，每次跳转时先用popToFirstView()清除除了第一个视图外的所有视图，再进行跳转。