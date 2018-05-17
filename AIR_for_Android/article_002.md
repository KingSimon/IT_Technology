# 在移动设备应用程序中使用软键盘

[TOC]

许多设备都不包括硬件键盘。在必要时，这些设备会使用在屏幕中打开的键盘。软键盘又称做屏幕键盘或虚拟键盘，当用户输入信息或取消操作后，软键盘将会关闭。

下图显示了一个使用了软键盘的应用程序：

![](http://help.adobe.com/zh_CN/flex/mobileapps/images/sk_open_keyboard_sk.png)

根据启用软键盘的组件，软键盘有不同的功能集：

*   本机功能：用于默认文本输入控件 TextArea 和 TextInput 的键盘，它挂接到本机接口，具有自动纠正、自动完成和自定义键盘布局等功能。对完整功能集的支持已内置到基于 StageText 默认外观类的文本输入控件中。并非所有设备都支持所有本机功能。

*   有限功能：用于 TextArea 和 TextInput 之外的任何其它控件的键盘，或者在控件使用基于 TextField 的外观时，用于 TextArea 和 TextInput 的键盘。有限功能集不支持本机 OS 功能，如自动纠正、自动完成和自定义键盘布局。

由于该键盘会占用一部分屏幕，Flex 必须确保应用程序在缩小的屏幕区域中仍能正常工作。例如，用户选择 TextInput 控件时会打开软键盘。键盘打开后，Flex 自动根据可用屏幕区域调整应用程序的大小。Flex 随后可以调整所选 TextInput 控件的位置，使其在键盘之上可见。

![](http://help.adobe.com/zh_CN/flex/mobileapps/images/byline.png) 博客 Peter Elst 发表了关于[在 Flex 移动设备应用程序中控制软键盘](http://www.peterelst.com/blog/2011/05/08/controlling-the-soft-keyboard-in-flex-mobile-applications/)的文章。

## 在移动设备 Flex 应用程序中打开软键盘

可使用三种方法在移动设备应用程序中打开软键盘：

*   将焦点放在具有[TextInput](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/components/TextInput.html)或[TextArea](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/components/TextArea.html)之类文本输入控件的控件上

*   将控件的`needsSoftKeyboard`属性设置为`true`，并将焦点放在该控件上

*   对控件调用`requestSoftKeyboard()`方法（不在 iOS 上）

键盘将保持打开状态，直至发生以下操作之一：

*   用户将焦点移动到不接收文本输入的控件。当用户手动指向另一个文本输入控件，或者用户按键盘上的返回键，应用程序随之将焦点移到另一个控件时就会发生这种情况。

    如果焦点移动到其他文本输入控件，或移动到`needsSoftKeyboard`属性为`true`的控件，则键盘保持打开状态。

*   用户按下设备上的后退按钮以取消输入。

*   您可通过编程将焦点更改到非交互式控件或将`stage.focus`设置为`null`。

**为用户提供文本输入控件**

如果为用户提供了文本输入控件，当用户将焦点放在该控件上时软键盘就会出现，除非将`editable`属性设置为`false`。

TextInput 和 TextArea 控件的默认行为是使用[StageText](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/text/StageText.html)类来呈现文本。因此，为这些控件显示的键盘支持自动纠正、自动大写和键盘类型等本机功能。并非所有功能在所有设备上都受支持。

如果将外观类更改为对文本输入控件使用基于[TextField](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/text/TextField.html)的外观，这些功能将受到限制。键盘本身相同，但不支持本机功能。

**设置`needsSoftKeyboard`属性**

可以配置非输入控件，以为其打开软键盘，例如 Button 或 ButtonBar 控件。要在文本输入控件外的其它控件获得焦点时打开键盘，请将该控件的`needsSoftKeyboard`属性设置为`true`。所有 Flex 组件都从[InteractiveObject](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/display/InteractiveObject.html)类继承此属性。

为 TextInput 和 TextArea 之外的任何控件打开的键盘都不支持自动大写、自动纠正和自定义键盘类型等本机功能。

`注：`当文本输入控件获得焦点时，始终会打开键盘。文本输入控件将忽略`needsSoftKeyboard`属性，设置该属性不会影响文本输入控件。

**调用`requestSoftKeyboard()`方法**

要以编程方式打开软键盘，可以调用`requestSoftKeyboard()`方法。对于调用此方法的对象，必须也将其`needsSoftKeyboard`属性设置为`true`。此方法会将焦点更改到调用此方法的对象上，如果设备没有硬件键盘，则会打开软键盘。

[InteractiveObject](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/display/InteractiveObject.html)类定义了`requestSoftKeyboard()`方法。因此，您可以在作为 InteractiveObject 子类的任何组件上调用此方法。

如果在 TextArea 或 TextInput 控件上调用`requestSoftKeyboard()`方法，则自动纠正和自动大写等本机键盘功能将是受支持的（如果设备支持这些功能）。

`requestSoftKeyboard()`方法不适用于 iOS 设备。

## 使用软键盘的本机功能

[StageTextInputSkin](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/skins/mobile/StageTextInputSkin.html)和[StageTextAreaSkin](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/skins/mobile/StageTextAreaSkin.html)类定义了移动设备应用程序中[TextInput](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/components/TextInput.html)和[TextArea](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/components/TextArea.html)控件的外观。这些外观使用[StageText](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/text/StageText.html)类来呈现文本，并挂接到软键盘的本机功能中。这些功能包括：

*   自动纠正

*   自动大写

*   自定义返回键标签

*   自定义键盘类型

文本输入控件也可以使用基于[TextField](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/text/TextField.html)的外观来呈现文本。这些外观不支持本机功能。但是，它们为基础文本控件提供了附加功能，如支持滚动表单、嵌入字体、访问`keyUp`和`keyDown`事件、剪切、文本测量和小数 Alpha 值。

要使用基于 TextField 的外观，请将文本输入控件的`skinClass`属性设置为指向[TextInputSkin](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/skins/mobile/TextInputSkin.html)和[TextAreaSkin](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/skins/mobile/TextAreaSkin.html)类。例如：

```xml
<s:TextInput skinClass="spark.skins.mobile.TextInputSkin"/>
<s:TextArea skinClass="spark.skins.mobile.TextAreaSkin"/>
```

### 在 Flex 移动设备应用程序中对软键盘使用自动纠正功能

自动纠正是 OS 试图纠正拼写错误并对用户输入应用预测性键入的一种行为。根据设备类型，此行为可能以文本之上的气泡提示框、软键盘扩展或其它某种方式实现。

您可以通过将文本输入控件的`autoCorrect`属性设置为`true`来在移动设备应用程序中对软键盘使用自动纠正功能。这是默认值。

在下例中您可以打开和关闭自动纠正功能。

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- mobile_keyboard/views/AutoCorrectionExample.mxml -->
<s:View xmlns:fx="http://ns.adobe.com/mxml/2009"
        xmlns:s="library://ns.adobe.com/flex/spark" title="Auto-Correction">
    <fx:Declarations>
        <!-- Place non-visual elements (e.g., services, value objects) here -->
    </fx:Declarations>
    <s:layout>
        <s:VerticalLayout/>
    </s:layout>
    <s:TextInput prompt="Enter your text" autoCorrect="{myCB.selected}"/> 
    <s:CheckBox id="myCB" label="Enable auto-correct" enabled="true"/>
</s:View>
```

并非所有设备都支持自动纠正功能。如果您在不支持此功能的设备上启用或禁用`autoCorrect`属性，运行时会忽略此值，并使用设备的默认行为。

### 在 Flex 移动设备应用程序中对软键盘使用自动大写功能

自动大写是一种在用户输入文本时指示文本输入控件将某些字或字母设置为大写的设置。例如，您可以将所有字母设为大写，也可以自动地只将每个句子的第一个字设为大写。这样用户在移动设备应用程序中输入文本时就不必担心大写设置了，这十分方便。

您可通过在文本输入控件上设置`autoCapitalize`属性值来在软键盘中使用自动大写功能。可能值有`none`、`word`、`sentence`和`all`。[AutoCapitalize](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/text/AutoCapitalize.html)类定义了可能的值。默认值为`none`。

在下例中您可以为自动大写功能选择不同的值。

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- mobile_keyboard/views/AutoCapitalizeExample.mxml -->
<s:View xmlns:fx="http://ns.adobe.com/mxml/2009"
        xmlns:s="library://ns.adobe.com/flex/spark" title="Auto-Capitalization">
    <fx:Declarations>
        <!-- Place non-visual elements (e.g., services, value objects) here -->
    </fx:Declarations>
    <s:layout>
        <s:VerticalLayout/>
    </s:layout>
    <s:Label text="Select a capitalization setting:"/>
    <s:SpinnerListContainer>
        <s:SpinnerList id="capTypeList" width="300" labelField="name" fontSize="12">
            <s:ArrayCollection>
                <fx:Object name="All" value="all"/>
                <fx:Object name="None" value="none"/>
                <fx:Object name="Sentence" value="sentence"/>
                <fx:Object name="Word" value="word"/>
            </s:ArrayCollection>
        </s:SpinnerList>
    </s:SpinnerListContainer>
    <s:TextInput autoCapitalize="{capTypeList.selectedItem.value}"/>
</s:View>
```

并非所有设备都支持自动大写功能。如果您在不支持此功能的设备上设置`autoCapitalize`属性的值，运行时会忽略此值，并使用设备的默认值。

### 在 Flex 移动设备应用程序中更改软键盘类型

[SoftKeyboardType](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/text/SoftKeyboardType.html)`类定义了移动设备应用程序的软键盘类型。您可在文本输入控件上使用`softKeyboardType`属性选择键盘类型。

大多数键盘（如`email`和`contact`）之间只有细微差别。例如，除以“@”代替了话筒外，`email`键盘与`contact`键盘为用户提供的按键完全相同。`url`键盘使用“/”符号。`number`键盘与众不同。此键盘外观类似一个计算器屏幕，焦点在数字和运算符上。

下例显示了可用的不同类型的软键盘：

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- mobile_keyboard/views/KeyboardTypes.mxml -->
<s:View xmlns:fx="http://ns.adobe.com/mxml/2009"
        xmlns:s="library://ns.adobe.com/flex/spark" title="Keyboard Types">
    <fx:Declarations>
        <!-- Place non-visual elements (e.g., services, value objects) here -->
    </fx:Declarations>
    <s:layout>
        <s:VerticalLayout/>
    </s:layout>
    <s:Label text="Select a keyboard type:"/>
    <s:SpinnerListContainer>
        <s:SpinnerList id="keyboardTypeList" width="300" labelField="name">
            <s:ArrayCollection>
                <fx:Object name="Contact" value="contact"/>
                <fx:Object name="Default" value="default"/>
                <fx:Object name="Email" value="email"/>
                <fx:Object name="Number" value="number"/>
                <fx:Object name="Punctuation" value="punctuation"/>
                <fx:Object name="URL" value="url"/>
            </s:ArrayCollection>
        </s:SpinnerList>
    </s:SpinnerListContainer>
    <s:TextInput softKeyboardType="{keyboardTypeList.selectedItem.value}" text=""/>
</s:View>
```

并非所有软键盘类型在所有设备上都受支持。如果指定了不受支持的类型，运行时会忽略此值，而使用设备的默认值。

### 在 Flex 移动设备应用程序中更改软键盘上的返回键标签

软键盘弹出，用户输入文本时，必须有一种方法让用户表示他们已完成了输入，并且希望移到下一个字段或提交所输入的数据。在软键盘上，通常使用“返回键”完成此操作。此按键不会在文本输入中输入任何字符，只是通知文本输入控件用户已完成了文本输入。

[ReturnKeyLabel](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/text/ReturnKeyLabel.html)类为返回键定义了可能的标签。可能的值有`default`、`done`、`go`、`next`和`search`。您可以在文本输入控件中使用`returnKeyLabel`属性指定返回键标签。

在下例中您可以选择不同的返回键标签：

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- mobile_keyboard/views/ReturnKeyLabels.mxml -->
<s:View xmlns:fx="http://ns.adobe.com/mxml/2009"
        xmlns:s="library://ns.adobe.com/flex/spark" title="Return Key Labels">
    <fx:Declarations>
        <!-- Place non-visual elements (e.g., services, value objects) here -->
    </fx:Declarations>
    <s:layout>
        <s:VerticalLayout/>
    </s:layout>
    <s:Label text="Select a return key label:"/>
    <s:SpinnerListContainer>
        <s:SpinnerList id="returnKeyLabelList" width="300" labelField="name">
            <s:ArrayCollection>
                <fx:Object name="Default" value="default"/>
                <fx:Object name="Done" value="done"/>
                <fx:Object name="Go" value="go"/>
                <fx:Object name="Next" value="next"/>
                <fx:Object name="Search" value="search"/>
            </s:ArrayCollection>
        </s:SpinnerList>
    </s:SpinnerListContainer>
    <s:TextInput returnKeyLabel="{returnKeyLabelList.selectedItem.value}" text=""/>
</s:View>
```

在事件或交互方面，不同的返回键类型之间没有差别。更改`returnKeyLabel`属性只会更改按键的标签。

并非所有设备都支持返回键标签的设置。如果您在不支持此功能的设备上设置`returnKeyLabel`属性的值，运行时会忽略此值，而使用设备的默认值。

## 在移动设备应用程序中使用与软键盘相关的事件

在移动设备上与软键盘进行交互不同于在桌面或基于 Web 的应用程序中与键盘进行交互。下表列出了与使用软键盘相关的事件：

| 事件 | 何时分派 |
|--------|--------|
|`enter`|用户按返回键时。|
|`keyDown`和`keyUp`|对于基于 StageText 的外观，仅在按下和释放某些按键时。对于基于 TextField 的外观，在按下和释放所有按键时。不会为所有设备上的所有按键分派这些事件。不要依赖这些方法来捕获软键盘的按键输入，除非对启用键盘的控件使用了基于 TestField 的外观。|
|`softKeyboardActivating`|打开键盘之前。|
|`softKeyboardActivate`|打开键盘之后。|
|`softKeyboardDeactivate`|关闭键盘之后。|

要确定用户何时在软键盘上完成了工作，可以在文本输入控件上侦听`FlexEvent.ENTER`事件。按下返回键时控件就会分派此事件。通过侦听`enter`事件，您可以执行验证、更改焦点，或对最近输入的文本执行其它操作。

在某些情况下，不会分派`enter`事件。在 Android 设备上当对视图中的最后一个文本输入控件使用软键盘时，此限制就会出现。要解决此问题，请在视图的最后一个文本输入控件上将`returnKeyLabel`属性设置为`go`、`next`或`search`。

在下例中，当用户在软键盘上按“Next”键时，焦点将从一个字段更改到下一个字段。

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- mobile_keyboard/views/UseNextLikeTab.mxml -->
<s:View xmlns:fx="http://ns.adobe.com/mxml/2009"
        xmlns:s="library://ns.adobe.com/flex/spark" title="Change Focus">
    <fx:Declarations>
        <!-- Place non-visual elements (e.g., services, value objects) here -->
    </fx:Declarations>
    <s:layout>
        <s:VerticalLayout paddingTop="10" paddingLeft="10" paddingRight="10"/>
    </s:layout>
    <fx:Script>
        <![CDATA[
            private function changeField(ti:TextInput):void {
                // Before changing focus to a new control, set the stage's focus to null:
                stage.focus = null;
                // Set focus on the TextInput that was passed in:
                ti.setFocus();
            }
        ]]>
    </fx:Script>
    <s:HGroup>
        <s:Label text="1:" paddingTop="15"/>
        <s:TextInput id="ti1" prompt="First Name"
                     width="80%"
                     returnKeyLabel="next"
                     enter="changeField(ti2)"/>
    </s:HGroup>
    <s:HGroup>
        <s:Label text="2:" paddingTop="15"/>
        <s:TextInput id="ti2" prompt="Middle Initial"
                     width="80%"
                     returnKeyLabel="next"
                     enter="changeField(ti3)"/>
    </s:HGroup>
    <s:HGroup>
        <s:Label text="3:" paddingTop="15"/>
        <s:TextInput id="ti3" prompt="Last Name"
                     width="80%"
                     returnKeyLabel="next"
                     enter="changeField(ti1)"/>
    </s:HGroup>
</s:View>
```

当用户与 TextInput 和 TextArea 控件的默认软键盘交互时，仅对一小部分按键分派`keyUp`和`keyDown`事件。要捕获所有键的各个按键行为，请使用`change`事件。只要文本输入控件的内容有更改，就会分派`change`事件。这样做的缺点是您无法访问按下的按键的属性，并且必须编写自己的按键逻辑。

下例显示了最后一个按下的按键的字符代码：

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- mobile_keyboard/views/CompareMobileKeyPresses.mxml -->
<s:View xmlns:fx="http://ns.adobe.com/mxml/2009" 
        xmlns:s="library://ns.adobe.com/flex/spark" title="Keyboard Events">
    <fx:Declarations>
        <!-- Place non-visual elements (e.g., services, value objects) here -->
    </fx:Declarations>
    <s:layout>
        <s:VerticalLayout/>
    </s:layout>
    <fx:Script>
        <![CDATA[
            private var storedValueOfText:String = null;

            // Compare the new text against the stored value to see what key was pressed.
            private function compareKey(e:Event):void {
                var key:String = "";
                if (storedValueOfText == null) {
                    key = ti1.text.charCodeAt(0).toString(); // Capture the first key pressed.
                } else {
                    // Compare the stored value against the current value and extract the difference.
                    for (var i:int = 0; i<ti1.text.length; i++) {
                        if (ti1.text.charAt(i) == storedValueOfText.charAt(i)) {
                            // Do nothing if they're equal.
                        } else {
                            key = ti1.text.charCodeAt(i).toString();
                        }
                    }
                }
                ti2.text = "The '" + key + "' key was pressed.";
                storedValueOfText = ti1.text;
            }
        ]]>
    </fx:Script>
    <s:TextInput id="ti1" change="compareKey(event)"/>
    <s:TextInput id="ti2" editable="false"/>

</s:View>
```

如果光标位于文本输入字段的末尾，此示例仅识别最后一个按下的按键。

当用户与基于 TextField 控件的软键盘进行交互时，像`keyUp`和`keyDown`这样的事件将适用于所有按键。下例使用`keyUp`处理函数来获取当前按键，并基于按键代码对 Label 控件应用样式。因为`requestSoftKeyboard()`方法会为 Label 控件而不是 TextInput 或 TextArea 控件启用键盘，应用程序将使用基于 TextField 的键盘。

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- mobile_text/views/RequestSoftKeyboardExample.mxml -->
<s:View xmlns:fx="http://ns.adobe.com/mxml/2009"
        xmlns:s="library://ns.adobe.com/flex/spark" title="requestSoftKeyboard()">
    <fx:Declarations>
        <!-- Place non-visual elements (e.g., services, value objects) here -->
    </fx:Declarations>
    <s:layout>
        <s:VerticalLayout/>
    </s:layout>
    <fx:Script>
        <![CDATA[
            private function handleButtonClick():void {
                myLabel.requestSoftKeyboard();
            }
            /* Ok to use keyUp handler on limited screen keyboard. */
            private function handleKeys(event:KeyboardEvent):void {
                var c:int;
                switch(event.keyCode) {
                    case(82): // 82 = "r"
                        c = 0xFF0000;
                        break;
                    case(71): // 71 = "g"
                        c = 0x00FF00;
                        break;
                    case(66): // 66 = "b"
                        c = 0x0000FF;
                        break;
                }
                event.currentTarget.setStyle("color",c);
            }
        ]]>
    </fx:Script>
    <s:Label id="myLabel" text="This is a label." needsSoftKeyboard="true" keyUp="handleKeys(event)"/>
    <s:Button id="b1" label="Click Me" click="handleButtonClick()"/>
</s:View>
```

要以编程方式关闭软键盘，请将`stage.focus`设为`null`。要关闭软键盘并将焦点设置到另一个控件，请将`stage.focus`设为`null`，然后将焦点设置在目标控件上。您也可以调用另一个控件的`requestSoftKeyboard()`方法来更改焦点，在另一个控件上打开软键盘。

可通过事件处理函数访问软键盘的某些属性。要访问软键盘的大小和位置，请使用[flash.display.Stage](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/flash/display/Stage.html)类的`softKeyboardRect`属性，如下例所示：

```xml
!-- mobile_keyboard/views/SoftKeyboardEvents.mxml -->
<s:View xmlns:fx="http://ns.adobe.com/mxml/2009"
        xmlns:s="library://ns.adobe.com/flex/spark" title="Soft Keyboard Events">
    <fx:Declarations>
        <!-- Place non-visual elements (e.g., services, value objects) here -->
    </fx:Declarations>
    <s:layout>
        <s:VerticalLayout/>
    </s:layout>
    <s:TextInput prompt="Enter your text"
             softKeyboardActivate="ta1.text+=stage.softKeyboardRect + '\n'"
             softKeyboardDeactivate="ta1.text+=stage.softKeyboardRect + '\n'"
             softKeyboardActivating="ta1.text+=stage.softKeyboardRect + '\n'"/>

    <s:TextArea id="ta1" width="100%" height="100%" editable="false"/>
</s:View>
```

## 配置应用程序以支持软键盘

为支持软键盘，当键盘打开时，应用程序应当可以执行以下操作：

*   根据剩余可用屏幕空间来调整应用程序大小，以免键盘会与应用程序发生重叠。

*   滚动获得焦点的文本输入控件的父容器，以确保该控件可见。


### 配置系统以支持软键盘

在全屏模式下运行的应用程序不支持软键盘。因此，请确保 app.xml 文件中的`<fullScreen>`属性设置为默认值`false`。

请确保应用程序的呈示模式设置为 CPU 模式。呈示模式由应用程序 app.xml 描述符文件中的`<renderMode>`属性控制。请确保`<renderMode>`属性设置为默认值`cpu`，而不是`gpu`。

`注：`默认情况下，app.xml 文件中不包括`<renderMode>`属性。要更改其设置，请将其添加为`<initialWindow>`属性中的一个条目。如果 app.xml 文件中未包含此设置，则其默认值为`cpu`。

### 打开软键盘时滚动父容器

`Application`容器的[resizeForSoftKeyboard](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/components/Application.html)属性决定着应用程序的大小调整行为。如果`resizeForSoftKeyboard`属性是默认值`false`，则键盘可以显示在应用程序的顶部。如果值为`true`，则会调整应用程序大小，以减去键盘大小。

要支持滚动，文本输入控件必须使用基于 TextField 的外观，而不是基于 StageText 的外观。可通过相应地将 TextInput 或 TextArea 控件的`skinClass`属性指向[TextInputSkin](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/skins/mobile/TextInputSkin.html)或[TextAreaSkin](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/skins/mobile/TextAreaSkin.html)类来达到此目的。

要进行滚动，请将任何文本输入控件的父容器封装在[Scroller](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/components/Scroller.html)组件中。当打开键盘的组件获得焦点时，Scroller 自动将该组件滚动到视图中。该组件也可以是 Scroller 组件的多个嵌套容器的子代。

父容器必须是[GroupBase](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/components/supportClasses/GroupBase.html)或[SkinnableContainer](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/components/SkinnableContainer.html)类或其子类。获得焦点的组件必须实现 IVisualElement 接口，并且必须可以获得焦点。

通过将父容器封装在 Scroller 组件中，可以在键盘打开的情况下滚动该容器。例如，某个容器中包含多个文本输入控件。然后，您滚动到每个文本输入控件以输入数据。

当关闭键盘时，父容器可能小于可用屏幕空间。如果容器小于可用屏幕空间，Scroller 会将滚动位置恢复为 0，即容器的顶部。

下面的示例中显示的是带有多个 TextInput 控件和一个 Scroller 组件的 View 容器。

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- containers\mobile\views\SparkMobileKeyboardHomeView.mxml -->
<s:View xmlns:fx="http://ns.adobe.com/mxml/2009"
        xmlns:s="library://ns.adobe.com/flex/spark"
        title="Compose Email">
    <s:Scroller width="100%" top="10" bottom="50">
        <s:VGroup paddingTop="3" paddingLeft="5" paddingRight="5" paddingBottom="3">
            <!-- Use TextField-based skins so that scrolling works. -->
            <s:TextInput prompt="To" width="100%" skinClass="spark.skins.mobile.TextInputSkin"/>
            <s:TextInput prompt="CC" width="100%" skinClass="spark.skins.mobile.TextInputSkin"/>
            <s:TextInput prompt="Subject" width="100%" skinClass="spark.skins.mobile.TextInputSkin"/>
            <s:TextArea height="400" width="100%" prompt="Compose Mail" skinClass="spark.skins.mobile.TextAreaSkin"/>
        </s:VGroup>
    </s:Scroller>
    <s:HGroup width="100%" gap="20"
              bottom="5" horizontalAlign="left">
        <s:Button label="Send" height="40"/>
        <s:Button label="Cancel" height="40"/>
    </s:HGroup>
</s:View>
```

VGroup 容器是 TextInput 控件的父容器。Scroller 将 VGroup 封装起来，每个 TextInput 控件在获得焦点时，都将显示在键盘上方。

有关 Scroller 组件的更多信息，请参阅[Scrolling Spark containers](http://help.adobe.com/zh_CN/flex/using/WSC5AF600B-9793-4dfd-AD9D-B9FC098869A8.html)。

### 打开软键盘时调整应用程序大小

如果[Application](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/components/Application.html)容器的`resizeForSoftKeyboard`属性为`true`，则应用程序会调整自身大小以在键盘打开时适合可用的屏幕区域。当关闭键盘时，应用程序恢复原有大小。

下例中显示的是一个应用程序的主应用程序文件，该应用程序通过将`resizeForSoftKeyboard`属性设置为`true`来允许调整应用程序大小。

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- containers\mobile\SparkMobileKeyboard.mxml -->
<s:ViewNavigatorApplication xmlns:fx="http://ns.adobe.com/mxml/2009"
    xmlns:s="library://ns.adobe.com/flex/spark"
    firstView="views.SparkMobileKeyboardHomeView"
    resizeForSoftKeyboard="true">
</s:ViewNavigatorApplication>
```

要允许调整应用程序大小，请确保将应用程序 app.xml 描述符文件中`<softKeyboardBehavior>`属性设置为`none`。`<softKeyboardBehavior>`属性的默认值为`none`。此默认值将 AIR 配置为移动整个 Stage，来使获得焦点的文本组件可见。

尽管软键盘事件的可靠性足以保证大多时候的自动调整行为可以达到预期效果，但仍应尽量避免将关键的 UI 元素放在软键盘事件失败时软键盘可能遮盖到的位置。例如，请避免在视图的下半部分放置“确定”、“登录”或“提交”按钮。

当基于 StageText 的控件成为动画或参与动画时，运行时会临时将其替换为位图，以便文本与其他项目同步移动。如果该控件具有焦点，这将导致控件临时丢失焦点。在某些情况下，软键盘会在不隐藏软键盘的情况下隐藏或触发`softKeyboardDeactivate`事件。

尤其是当以编程方式将弹出窗口内的基于 StageText 的组件设为焦点，而由于软键盘导致窗口大小动态变化时，该问题特别明显。要避免此情况，请尽量在不会因软键盘而调整大小的弹出窗口内使用基于 StageText 的组件。

如果必须在可调整大小的弹出窗口中使用基于 StageText 的组件，请尝试首先显示软键盘并等待弹出窗口的动画完成，然后再以编程方式将基于 StageText 的组件设为焦点。作为替代方法，可以避免以编程方式在弹出窗口内设置焦点。


### 配置用于软键盘的弹出窗口

[Callout](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/components/Callout.as)容器作为弹出窗口显示在移动设备应用程序的顶部。Callout 容器可以包含一个或多个接受键盘输入的组件。有关 Callout 容器的更多信息，请参阅[使用 Callout 容器创建 callout](http://help.adobe.com/zh_CN/flex/mobileapps/WS65023b321a7cb20f-76d41e2e131d387df5a-7fff.html)。

使用[SkinnablePopUpContainer](http://help.adobe.com/zh_CN/FlashPlatform/reference/actionscript/3/spark/components/SkinnablePopUpContainer.html)容器（Callout 的父类）的属性配置弹出窗口与键盘的交互：

*   moveForSoftKeyboard

*   如果为默认值`true`，弹出窗口将在键盘打开时移至键盘上方。

*   resizeForSoftKeyboard

*   如果为默认值`true`，弹出窗口将在键盘打开时调整大小以适合键盘上方的可用空间。

如果 SkinnablePopUpContainer 的`moveForSoftKeyboard`或`resizeForSoftKeyboard`属性设置为`true`，每当软键盘的可见性发生变化时，该容器都可能移动或发生大小调整。这两种自动行为都可能导致其他组件移动或发生大小变化。

避免这种情况的最简单方法是不将`resizeForSoftKeyboard`设置为`true`。如果应用程序不会因软键盘而调整大小，则软键盘不会导致组件从用户手指下的位置移开。但是，这会导致软键盘遮盖一些应用程序的 UI。

如果应用程序需要自动调整大小，请置入交互式组件，这样，当在对其操作时，软键盘可见性的变化便不会导致其从用户手指的位置移开。要做到这一点，需要以下技术：

*   不要为按钮、复选框或其他较小目标组件设置下约束，也不要将这些组件放置在设置了百分比高度的其他组件下方。

*   尝试将布局设计为当显示或隐藏键盘时最底层组件的顶部保持固定。

*   对于内置功能不能实现隐藏软键盘的平台，请提供一个固定的 UI 元素来实现该操作。这可以是专用于隐藏软键盘的按钮，也可以仅仅是足够大的固定空白边缘（可以轻敲该边缘以将焦点从文本组件移开）。

*   不要垂直排列可能会移动的组件。这么做可能导致在软键盘的可见性发生变化时手势定位到错误的目标。