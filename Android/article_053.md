# 判断SD卡容量是否已满
```
public static boolean isEnoughSpace(long size){
   if(Environment.getExternalStorageDirectory().equals
       (Environment.MEDIA_MOUNTED)){
      File path = Environment.getExternalStorageDirectory();
      StatFs statFs = new StatFs(path.getPath());
      long blockSize = statFs.getBlockSize();
      long availableBlocks = statFs.getAvailableBlocks();
      if(size < availableBlocks *  blockSize ){
         return true;
      }else{
         return false;
      }
   }
   return false;
}
```