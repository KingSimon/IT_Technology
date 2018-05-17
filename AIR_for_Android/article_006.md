# Mobile Flex 手机屏幕滑动事件
mxml格式：gestureSwipe="onSwipe(event)"
as格式：this.addEventListener(TransformGestureEvent.GESTURE_SWIPE,onSwipe);

```
/**
* 手势事件
*/
protected function onSwipe(event:TransformGestureEvent):void
{
    switch(event.offsetX)
    {
        case 1:
        {
            if(currentPage +1 == 2)
            {
                currentPage = 0;
            }
            else
            {
                currentPage++;
            }
            break;
        }
        case -1:
        {
            if(currentPage - 1 < 0)
            {
                currentPage = 1;
            }
            else
            {
                currentPage--;
            }
            break;
        }
    }
}
<s:states>
    <s:State name="page_one"/>
    <s:State name="page_two"/>
</s:states>
<s:transitions> 
    <s:Transition id="myTransition" fromState="*" toState="*">
        <s:Parallel id="t1" targets="{[grp_page1,grp_page2]}">
            <s:Fade duration="400"/>
        </s:Parallel>
    </s:Transition>
</s:transitions>
//注：其中grp_page1: visible.page_one = false visible.page_two = true 
//grp_page2: visible.page_one = true visible.page_two = false
```
