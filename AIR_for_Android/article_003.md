# Mobile Flex聊天功能（支持表情显示）

[TOC]

## ChatTextBox.mxml    表情文本显示

```
<?xml version="1.0" encoding="utf-8"?>  
<s:VGroup xmlns:fx="http://ns.adobe.com/mxml/2009"
          xmlns:s="library://ns.adobe.com/flex/spark"
          gap="0" horizontalAlign="left" verticalAlign="middle"
          minWidth="0" maxWidth="{forceWidth}">  
    <fx:Script>  
        <![CDATA[  
            import com.person.model.modelData.ModelFaces;  
 
            import mx.core.UIComponent;  
            import mx.events.FlexEvent;  
 
            import spark.components.HGroup;  
            import spark.components.Image;  
 
            private var currentHB:HGroup;  
            private var currentInLineWidth:Number = 0;  
            private var maxText:int = 100;  
            private var sendTxtArr:Array=[];  //type: 1,text 2,img   
            private var leftStr:String = "[";  
            private var rightStr:String = "]";  
            private var currentIndex:Number = -1;  
 
 
            private var muiltTextArr:Array =[];  
 
            public var lineSpace:Number = 2;  
            public var bold:String='normal';  
            public var italic:String="normal";  
            public var underline:String="none";  
            public var color:uint = 0x000000;  
 
            [Bindable]  
            public var forceWidth:Number = 210;  
            public var forceImageSize:Number = 30;  
            public var fontSize:Number = 18;  
 
 
            /**  
             * 信息分类处理方法  
             */
            public function splitString(txt:String):void
            {     
                var a1:Array=txt.split(leftStr);  
                for(var i:int=0; i < a1.length; i++)  
                {  
                    if (i == 0)  
                    {  
                        sendTxtArr.push({label:a1[i], type:'1'});  
                    }  
                    else
                    {  
                        var s1:String=a1[i].toString()  
                        if (s1.indexOf(rightStr) != -1)  
                        {  
                            var s2:String=s1.substring(0, s1.indexOf(rightStr));  
 
                            if (s2.length <= 2)  
                            {  
                                var smile:String = leftStr + s2 + rightStr;  
                                for each (var smiley:Object in ModelFaces.getInstance().faces)   
                                {  
                                    if (smile == smiley.imageName)  
                                    {  
                                        sendTxtArr.push({label:smiley.imageUrl, type:'2'});  
                                        var s3:String=s1.substring(s1.indexOf(rightStr) + 1, s1.length);  
                                        if(s3.length>0)  
                                        {  
                                            sendTxtArr.push({label:s3, type:'1'});  
                                        }  
                                    }  
                                }  
                            }  
 
                        }  
                        else
                        {  
                            sendTxtArr.push({label:leftStr + s1, type:'1'});  
                        }  
                    }  
                }  
 
                createChildQueue();  
            }  
 
            /**  
             * 创建子项队列方法  
             */
            private function createChildQueue():void
            {  
                ++currentIndex;  
                if (sendTxtArr[currentIndex] != undefined)  
                {  
                    if (Object(sendTxtArr[currentIndex]).type == '1')  
                    {  
                        addTxt(Object(sendTxtArr[currentIndex]).label)  
                    }  
                    else if (Object(sendTxtArr[currentIndex]).type == '2')  
                    {  
                        addImg(Object(sendTxtArr[currentIndex]).label)  
                    }  
 
                }  
                else
                {  
                    dispatchEvent(new Event('ChildCreateComplete'));  
                }  
            }  
 
            /**  
             * 添加表情  
             */
            private function addImg(url:Class):void
            {  
                addNewHBox();  
                var img:Image=new Image();  
                img.source= new url();  
                img.visible=false;  
                img.addEventListener(FlexEvent.UPDATE_COMPLETE, imgComplete,false,0,true);  
                currentHB.addElement(img);  
            }  
 
 
            /**  
             * 图片加载完成事件  
             */
            private function imgComplete(e:FlexEvent):void
            {  
                var img:Image=Image(e.currentTarget);  
                if (img.width == 0)  
                {  
                    return ;  
                }  
 
                if (img.width > forceImageSize)  
                {  
                    img.width = forceImageSize;  
                    img.height = forceImageSize;  
                }  
 
                if ((img.x + img.width) > forceWidth)  
                {  
                    currentHB.removeElement(img);  
                    addNewHBox(true);  
                    addImg(getDefinitionByName(getQualifiedClassName(img.source)) as Class);  
                }  
                else
                {  
                    img.visible = true;  
                    currentInLineWidth = img.x + img.width;  
                    createChildQueue();  
                    currentHB.invalidateProperties();  
                    currentHB.invalidateSize();  
                    currentHB.invalidateDisplayList();  
                }  
                img.smooth = true;  
            }  
 
 
            /**  
             *  添加文字  
             */
            public function addTxt(str:String):void
            {         
                if(str.length > maxText)  
                {  
                    str=str.substring(0,maxText)+"...";  
                }  
                addNewHBox();  
 
                if ((forceWidth - currentInLineWidth) <= fontSize)  
                {  
                    addNewHBox(true);  
                }  
 
                var t:TextField = new TextField();  
                t.width = forceWidth - currentInLineWidth;  
                t.visible = false;  
                t.text=str;  
                t.wordWrap = true;  
                var fm:TextFormat = new TextFormat();  
                fm.bold = bold == 'bold' ? true : false;  
                fm.italic = italic == "italic" ? true : false;  
                fm.underline = underline == "underline" ? true : false;  
                fm.color=color;  
                fm.size=fontSize;  
                t.setTextFormat(fm);  
                t.addEventListener(Event.ADDED_TO_STAGE, txtCreateComplete,false,0,true);  
                var ui:UIComponent=new UIComponent();  
                ui.addChild(t);  
                currentHB.addElement(ui);  
            }                 
 
            /**  
             *  文字加载完成事件  
             */
            private function txtCreateComplete(e:Event):void
            {  
                var t:TextField = TextField(e.currentTarget);  
                var fm:TextFormat = new TextFormat();  
                fm.color = color;  
                fm.size = fontSize;  
                t.setTextFormat(fm);      
                var ui:UIComponent = UIComponent(t.parent);  
                ui.width = t.textWidth;  
                ui.height= t.textHeight/t.numLines + lineSpace;  
                if (t.numLines > 1)  
                {  
                    var out0:String="";  
                    var out:String='';  
                    for(var i:int=0; i < t.numLines; i++)  
                    {                                         
                        if (i != 0)  
                        {                         
                            out += t.getLineText(i);                                              
                        }  
                    }     
                    t.textColor = color;  
                    t.htmlText = t.getLineText(0);  
                    t.visible = true;  
                    muiltTextAddRow(out);  
                    fm.size = fontSize;  
                    t.setTextFormat(fm);  
                }  
                else
                {  
                    t.visible=true;  
                    currentInLineWidth = ui.x + ui.width;  
                    createChildQueue();  
                }  
 
            }  
 
 
            /**  
             *  添加多行文本  
             */
            private function muiltTextAddRow(s:String):void
            {  
 
                addNewHBox(true);  
                var t:TextField=new TextField();  
                t.width = forceWidth - currentInLineWidth;  
                t.visible=false;  
                t.htmlText = s;  
                t.wordWrap=true;  
                var fm:TextFormat=new TextFormat();  
                fm.bold=bold == 'bold' ? true : false;  
                fm.italic=italic == "italic" ? true : false;  
                fm.underline=underline == "underline" ? true : false;  
                fm.color=color;  
                fm.size=fontSize;  
                t.setTextFormat(fm);  
                t.addEventListener(Event.ADDED_TO_STAGE, txtCreateComplete,false,0,true);  
                var ui:UIComponent=new UIComponent();  
                ui.addChild(t);  
                currentHB.addElement(ui);  
            }  
 
            /**  
             * 新建一行  
             * @param   bool: 是否强制新建一行  
             */
            private function addNewHBox(bool:Boolean=false):HGroup  
            {  
                if (bool)  
                {  
                    currentInLineWidth=0;  
 
                    var hb:HGroup = new HGroup();  
                    hb.verticalAlign = "middle";  
                    hb.horizontalAlign = "left";  
                    hb.gap = 0;  
                    this.addElement(hb);  
                    currentHB=hb;  
 
                    return currentHB;  
                }  
 
                if (currentHB == null)  
                {  
                    hb = new HGroup();  
                    hb.verticalAlign = "middle";  
                    hb.horizontalAlign = "left";  
                    hb.gap = 0;  
                    this.addElement(hb);  
                    currentHB = hb;  
                }  
                return currentHB;  
            }  
        ]]>  
    </fx:Script>  
    <fx:Declarations>  
        <!-- 将非可视元素（例如服务、值对象）放在此处 -->  
    </fx:Declarations>  
    <fx:Metadata>  
        [Event(name="ChildCreateComplete", type="flash.events.Event")]  
    </fx:Metadata>  
</s:VGroup>
```
## ChatMsgItem.mxml    聊天对话框
```
<?xml version="1.0" encoding="utf-8"?>  
<s:Group xmlns:fx="http://ns.adobe.com/mxml/2009"
         xmlns:s="library://ns.adobe.com/flex/spark"
         xmlns:items="com.person.views.items.*"
         width="100%">  
    <fx:Script>  
        <![CDATA[  
            import mx.events.FlexEvent;  
 
            import spark.events.ElementExistenceEvent;  
 
            [Bindable]  
            public var chatTime:String;  
 
            [Bindable]  
            public var isDisappear:Boolean = false;  
 
            [Bindable]  
            private var msg:String = "";  
 
            [Bindable][Embed(source="assets/person/person.swf",symbol="other")]  
            private var img_other:Class;  
            [Bindable][Embed(source="assets/person/person.swf",symbol="me")]  
            private var img_me:Class;  
 
            [Bindable]  
            private var bg_color:uint = 0xb4e447;  
 
 
            /**  
             *  设置信息方法  
             *  @param  msg 信息   
             *  @param  isSelf  是否是自己发的信息  
             */
            public function setMsg( msg:String,sendDate:String ,sendImg:String, isSelf:Boolean):void
            {  
                this.msg = msg;  
                if(isSelf)  
                {  
                    grp_main.right = 0;  
                    grp_item.swapElements(img_photo,grp_msg);  
                    img_pic.source = img_me;  
                    bg_color = 0xf3f3f3;  
                }  
                else
                {  
                }  
                img_photo.source = sendImg;  
                lab_date.text = sendDate;  
                content.splitString(msg);  
                this.visible = false;  
            }  
 
            public function uptext():void
            {  
                if(this.parentDocument.content.contentHeight > this.parentDocument.content.height )  
                {  
                    trace("msg.substr(msg.length-1,1) =" + msg.substr(msg.length-1,msg.length-1) );  
                    if(msg.substr(msg.length-1,msg.length-1) == "]")  
                    {  
                        this.parentDocument.content.verticalScrollPosition = this.parentDocument.content.contentHeight - this.parentDocument.content.height;  
                    }  
                    else
                    {  
                        this.parentDocument.content.verticalScrollPosition = this.parentDocument.content.contentHeight;  
                    }  
                }  
            }  
        ]]>  
    </fx:Script>  
    <fx:Declarations>  
        <!-- 将非可视元素（例如服务、值对象）放在此处 -->  
    </fx:Declarations>  
    <s:Label id="lab_date" height="20" width="100%" color="#5C5C5C" fontSize="14" textAlign="center"
             verticalAlign="middle"/>  
    <s:VGroup id="grp_main" top="2" gap="2" paddingLeft="10" paddingTop="20" paddingRight="10">  
        <s:HGroup id="grp_item" width="100%" gap="-1">  
            <s:BitmapImage id="img_photo" width="48" height="48" smooth="true">  
                <s:mask>  
                    <s:BorderContainer width="48" height="48" cornerRadius="5"/>  
                </s:mask>  
            </s:BitmapImage>  
            <s:VGroup id="grp_pic" width="10" height="48" depth="1" horizontalAlign="right"
                      verticalAlign="middle">  
                <s:BitmapImage id="img_pic" width="10" fillMode="scale" source="{img_other}"/>  
            </s:VGroup>    
            <s:Group id="grp_msg" height="90%">  
                <s:BorderContainer width="100%" height="100%" backgroundColor="{bg_color}"
                                   cornerRadius="8" dropShadowVisible="true" verticalCenter="0"/>  
                <s:Group left="6" right="8" top="5" bottom="5" height="100%">  
                    <items:ChatTextBox id="content" width="100%" height="100%" ChildCreateComplete="this.visible = true;uptext()"/>  
                </s:Group>  
            </s:Group>    
        </s:HGroup>    
    </s:VGroup>  
</s:Group>
```
## ChatMessageShowView.mxml    显示面板框
```
<?xml version="1.0" encoding="utf-8"?> 
<s:SkinnableContainer xmlns:fx="http://ns.adobe.com/mxml/2009"
                      xmlns:s="library://ns.adobe.com/flex/spark"> 
    <fx:Script> 
        <![CDATA[ 
 
            /** 
             *  控制文字输入后置底 
             */
            [Bindable] 
            public function updateText():void
            { 
                 if(content.contentHeight > content.height) 
                { 
                    content.verticalScrollPosition = content.contentHeight - content.height; 
                }  
            } 
        ]]> 
 
    </fx:Script> 
    <s:layout> 
        <s:VerticalLayout/> 
    </s:layout> 
    <s:Group bottom="10" width="100%" height="100%"> 
        <s:Rect id="backgroundRect" left="0" right="0" top="0" bottom="0"> 
            <s:fill> 
                <s:SolidColor color="#f3f1da"/> 
            </s:fill> 
        </s:Rect>   
    <s:Scroller id="chatMessageScroller" width="100%" height="100%" > 
        <s:VGroup id="content" width="100%" height="100%" gap="10" hasFocusableChildren="true"
                  paddingBottom="10" verticalAlign="top" resize="updateText()"/> 
    </s:Scroller>    
    </s:Group> 
</s:SkinnableContainer>
```