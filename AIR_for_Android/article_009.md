# Flex4.5移动开发 保存文件与打开文件
```actionscript
   private function SaveHtml(htmls:String):void
   {
 
    var fname:String="samples/"+data.title+".html";
    var file:File = File.userDirectory;
    file = file.resolvePath(fname);
    trace("_________nativePath: " + file.nativePath + "  url: " + file.url);
 
    fileurl=file.url;   
    if(file.exists)     
    {
 
    }
    else
    {    
    // if (file.exists) //检查文件是否存在，根据这个可以实现是否覆盖原来的文件，检查原来是否有该文件有的话再保存，根据自己需求吧
    var filebyte:ByteArray=new ByteArray();
    filebyte.writeUTFBytes(htmls);//直接写入utf字节流
    var stream:FileStream = new FileStream();
    stream.open(file,FileMode.WRITE);
    stream.writeBytes(filebyte,0,filebyte.length);
    stream.close();
    trace("保存成功");
    }
   }
 
 
 
   protected function button2_clickHandler(event:MouseEvent):void
   {
    // TODO Auto-generated method stub
 
    navigateToURL(new URLRequest(fileurl));     
   }

```