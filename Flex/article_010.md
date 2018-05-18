# Flex 修改鼠标样式
```actionscript
[Embed(source="../../Assets/kxyk/wq (11).gif")]
static public const wq1:Class;
[Embed(source="../../Assets/kxyk/wq (45).gif")]
static public const wq2:Class;
[Embed(source="../../Assets/kxyk/wq (60).gif")]
static public const wq3:Class;


private var _currentCursor:Class = null;
protected function setCursorIcon(icon:Class):void {
//设置鼠标光标样式
    _currentCursor = icon;
    CursorManager.removeAllCursors();
    if (_currentCursor) {
        CursorManager.setCursor(_currentCursor, 2, 3, 3);
    }
}
protected function setDefault():void {
//清空鼠标光标样式
    setCursorIcon(null);
}
```