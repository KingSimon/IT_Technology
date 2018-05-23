# 定位光标到特定字符以及复制的实现
光标定位：
```
EditText editText = (EditText)findViewById(r.id.textId);
editText.setText("AAA");
editText.setSelection(3);
```
选择字符进行复制：
```
ClipboardManager clipboard=(ClipboardManager)getSystemService(CLIPBOARDSERVICE);
clipboard.setText("Text to copy");
String data=clipboard.getText();
boolean isData=clipboard.hasText();
```