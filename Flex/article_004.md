# Flexlite添加png jpg图片
直接将图片的class对像指定给UIAssets Button这种组件的skinName即可.如下代码所示:
```
public class Assets
{
      [Embed(source="icon_add_1616.png")]
      public static var icon_add_1616:Class;
}
```
新建一个Assets类.里面添加一个icon_add_1616的图标.使用如下:
```
var addUi : UIAssets = new UIAssets();
addUi.skinName = Assets.icon_add_1616;
```