# Flex的变化数值监听
使用ChangeWatcher
```actionscript
import mx.binding.utils.ChangeWatcher;

protected function creationComplete():void

{
    ChangeWatcher.watch(modelRegister, "registerStatus",nameSetter);
}

public function nameSetter(event:Event):void
{
    trace(modelRegister.registerStatus);
    if(modelRegister.registerStatus == "registerOK")
    {
        this.showMsg.text = "registerOK";
    }
    else if(modelRegister.registerStatus == "registerFail")
    {
        this.showMsg.text = "registerFail";
    }
}
```