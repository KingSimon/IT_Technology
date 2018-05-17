# selectedIndex 失效问题
```actionscript
var dp:* = TabbedViewNavigator.navigators ;
TabbedViewNavigator.navigators = null;
TabbedViewNavigator.navigators = dp;
TabbedViewNavigator.selectedIndex  = data.selectedIndex;
TabbedViewNavigator.validateNow();
```