# Parsley框架的函数注入
RegisterServiceImpl.as
```
public function returnRegisterResult(result:String):void {
    trace("RegisterServiceImpl.returnRegisterResult(result)");
    if(result == "success") {
        RegisterModel.getInstance().registerStatus = "registerOK"
        dispatch(new RegisterEvent(RegisterEvent.REGISTERSUCCESS));
    } else {
        dispatch(new RegisterEvent(RegisterEvent.REGISTERFAIL));
    }
}
```
RegisterEvent.as
```
package event {
    import flash.events.Event;

    public class RegisterEvent extends Event
    {
        public static const REGISTERSUCCESS:String = "registerSuccess";
        public static const REGISTERFAIL:String = "registerFail";
        public function RegisterEvent(type:String, bubbles:Boolean = false, cancelable:Boolean = false) {
            super(type, bubbles, cancelable);
        }
    }
}
```
RegisterView.mxml
```
[MessageHandler(selector="registerSuccess")]
public function onLoginSuccess(evt:RegisterEvent):void {
    var str:String = "register success";
    this.showMsg.text = str;
}

[MessageHandler(selector="registerFail")]
public function onLoginFail(evt:RegisterEvent):void {
    var str:String = "register fail.";
    this.showMsg.text = str;
}
```