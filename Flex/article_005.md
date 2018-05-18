# Flexlite框架自定义皮肤
```
package
{
    import org.flexlite.domUI.components.TitleWindow;
    import org.flexlite.domUI.managers.SystemManager;
    import org.flexlite.domUI.skins.vector.TitleWindowSkin;

    /**
     * TitleWindow测试
     * @author DOM
     */
    public class TitleWindowTest extends SystemManager
    {
        public function TitleWindowTest()
        {
            super();
            var window:TitleWindow = new TitleWindow();
            window.height = 300;
            window.width = 400;
            window.x = 30;
            window.y = 30;
            window.title = "测试窗口";
            window.skinName = TitleWindowSkin;//指定皮肤
            addElement(window);
        }
    }
}
```

注意这句：
`window.skinName = TitleWindowSkin;`
只要给组件的skinName指定一个皮肤类即可

每个例子的文档类都继承自AppContainer。而它的构造函数里有这么句：Injector.mapClass(Theme,VectorTheme);意思是配置全局主题为默认的VectorTheme,如果组件没有被显式指定skinName,就会调用你注入的Theme实例，为自己查询获取一个默皮肤。你在项目里也可以定义自己的主题类，为每个组件配置默认皮肤。写起来就可以直接省略赋值皮肤这一步了。并且当你要修改默认皮肤的时候，也只需改这一个地方即可。具体参考VectorTheme的写法。框架里没有默认启用VectorTheme是为了减少不必要的引用，从而减小编译大小。项目开发的时候建议加上那句，开启默认皮肤方便调试，等到发布的时候去掉就行了。