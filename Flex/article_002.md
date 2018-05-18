# 事件监听优化处理
组件mxml添加两个事件 add="add(event)" remove="remove(event)"

**注：ItemRenderer 无法监听到add和remove事件 所以无法使用该方法**

```actionscript
/**
*组件添加事件
*/

protectedfunction add(event:Event):void
{
    setEventListener(true);
}

/**
 *组件移除事件
 */
protectedfunction remove(event:Event):void
{
    setEventListener(false);
    System.gc();
}

/**
*设置事件监听
* @parambool添加还是移除监听
*/
publicfunction setEventListener(bool:Boolean):void
{
    if(bool)
    {
        var str:String = "addEventListener";
    }
    else
    {
        str = "removeEventListener";
    }

    //添加组件需要用的监听事件
    btn_back[str](MouseEvent.CLICK, btn_back_clickHandler);
    btn_msg[str](MouseEvent.CLICK, btn_msg_clickHandler);
    btn_send[str](MouseEvent.CLICK, btn_send_clickHandler);
    btn_change[str](MouseEvent.CLICK, btn_change_clickHandler);
    btn_extends[str](MouseEvent.CLICK, btn_extends_clickHandler);
    chatMsg[str](MouseEvent.CLICK, chatMsg_clickHandler);
    txt_input[str](SoftKeyboardEvent.SOFT_KEYBOARD_ACTIVATE,txt_number_ActivateHandler);
    txt_input[str](SoftKeyboardEvent.SOFT_KEYBOARD_DEACTIVATE,txt_number_DeactivateHandler);
    this[str](KeyboardEvent.KEY_DOWN,keyDown);
}

/**
*实体按键事件
*/
privatefunction keyDown(e:KeyboardEvent):void
{
    switch(e.keyCode)
    {
    case Keyboard.BACK:
        e.preventDefault();
        if(grp_extends.height == 0 && grp_face.height == 0)
        {
            btn_back_clickHandler(null);
        }
        else
        {
            chatMsg_clickHandler(null);
        }
        System.gc();
        break;
    case Keyboard.MENU:
        break;
    }
}
```
