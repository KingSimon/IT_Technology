# 使用单例模式实现自己的HttpClient工具类
## 引子

在Android开发中我们经常会用到网络连接功能与服务器进行数据的交互，为此Android的SDK提供了Apache的HttpClient来方便我们使用各种Http服务。你可以把HttpClient想象成一个浏览器，通过它的API我们可以很方便的发出GET，POST请求（当然它的功能远不止这些）。

比如你只需以下几行代码就能发出一个简单的GET请求并打印响应结果：


```
try {        // 创建一个默认的HttpClient
HttpClient httpclient =new DefaultHttpClient();        // 创建一个GET请求
// 发送GET请求，并将响应内容转换成字符串
HttpGet request =new HttpGet("www.google.com");
String response = httpclient.execute(request, new BasicResponseHandler());        Log.v("response text", response);
    } catch (ClientProtocolException e) {
        e.printStackTrace();
    } catch (IOException e) {
        e.printStackTrace();
    }
```

## 为什么要使用单例HttpClient？

这只是一段演示代码，实际的项目中的请求与响应处理会复杂一些，并且还要考虑到代码的容错性，但是这并不是本篇的重点。注意代码的第三行：
```
HttpClient httpclient =new DefaultHttpClient();
```
在发出HTTP请求前，我们先创建了一个HttpClient对象。那么，在实际项目中，我们很可能在多处需要进行HTTP通信，这时候我们不需要为每个请求都创建一个新的HttpClient。因为之前已经提到，HttpClient就像一个小型的浏览器，对于整个应用，我们只需要一个HttpClient就够了。看到这里，一定有人心里想，这有什么难的，用单例啊！！就像这样：


```
publicclass CustomerHttpClient {
    private static HttpClient customerHttpClient;
        private CustomerHttpClient() {    }
        publicstatic HttpClient getHttpClient() {
        if(null== customerHttpClient) {
            customerHttpClient =new DefaultHttpClient();
        }
        return customerHttpClient;
    }
}
```

那么，哪里不对劲呢？或者说做的还不够完善呢？

多线程！试想，现在我们的应用程序使用同一个HttpClient来管理所有的Http请求，一旦出现并发请求，那么一定会出现多线程的问题。这就好像我们的浏览器只有一个标签页却有多个用户，A要上google，B要上baidu，这时浏览器就会忙不过来了。幸运的是，HttpClient提供了创建线程安全对象的API，帮助我们能很快地得到线程安全的“浏览器”。

## 解决多线程问题


```
publicclass CustomerHttpClient {
    privatestaticfinal String CHARSET = HTTP.UTF_8;
    privatestatic HttpClient customerHttpClient;
    private CustomerHttpClient() {    }
    publicstaticsynchronized HttpClient getHttpClient() {
        if (null== customerHttpClient) {
            HttpParams params =new BasicHttpParams();
            // 设置一些基本参数
            HttpProtocolParams.setVersion(params, HttpVersion.HTTP_1_1);
            HttpProtocolParams.setContentCharset(params,CHARSET);
            HttpProtocolParams.setUseExpectContinue(params, true);
            HttpProtocolParams.setUserAgent(params,
"Mozilla/5.0(Linux;U;Android 2.2.1;en-us;Nexus OneBuild.FRG83) "
            +"AppleWebKit/553.1(KHTML,like Gecko) Version/4.0
           MobileSafari/533.1");

// 超时设置
/* 从连接池中取连接的超时时间 */
            ConnManagerParams.setTimeout(params, 1000);
            /* 连接超时 */
            HttpConnectionParams.setConnectionTimeout(params, 2000);
            /* 请求超时 */
            HttpConnectionParams.setSoTimeout(params, 4000);
                        // 设置我们的HttpClient支持HTTP和HTTPS两种模式
            SchemeRegistry schReg =new SchemeRegistry();
            schReg.register(new Scheme("http",PlainSocketFactory
                    .getSocketFactory(), 80));
            schReg.register(new Scheme("https", SSLSocketFactory
                    .getSocketFactory(), 443));
            // 使用线程安全的连接管理来创建HttpClient
            ClientConnectionManager conMgr =new ThreadSafeClientConnManager(
                    params, schReg);
            customerHttpClient =new DefaultHttpClient(conMgr, params);
        }
        return customerHttpClient;
    }
}

```
在上面的getHttpClient()方法中，我们为HttpClient配置了一些基本参数和超时设置，然后使用ThreadSafeClientConnManager来创建线程安全的HttpClient。上面的代码提到了3种超时设置，比较容易搞混，故在此特作辨析。

## HttpClient的3种超时说明

/* 从连接池中取连接的超时时间 */ConnManagerParams.setTimeout(params, 1000);/* 连接超时 */HttpConnectionParams.setConnectionTimeout(params, 2000);/* 请求超时 */HttpConnectionParams.setSoTimeout(params, 4000);


第一行设置ConnectionPoolTimeout：这定义了从ConnectionManager管理的连接池中取出连接的超时时间，此处设置为1秒。

第二行设置ConnectionTimeout：　　这定义了通过网络与服务器建立连接的超时时间。Httpclient包中通过一个异步线程去创建与服务器的socket连接，这就是该socket连接的超时时间，此处设置为2秒。

第三行设置SocketTimeout：　　　　这定义了Socket读数据的超时时间，即从服务器获取响应数据需要等待的时间，此处设置为4秒。

　　

以上3种超时分别会抛出ConnectionPoolTimeoutException,ConnectionTimeoutException与SocketTimeoutException。　

## 封装简单的POST请求

有了单例的HttpClient对象，我们就可以把一些常用的发出GET和POST请求的代码也封装起来，写进我们的工具类中了。目前我仅仅实现发出POST请求并返回响应字符串的方法以供大家参考。将以下代码加入我们的CustomerHttpClient类中：


```
privatestaticfinal String TAG ="CustomerHttpClient";
publicstatic String post(String url, NameValuePair... params) {
        try {
            // 编码参数
            List<NameValuePair> formparams =new ArrayList<NameValuePair>();
           // 请求参数
            for (NameValuePair p : params) {
                formparams.add(p);
            }
            UrlEncodedFormEntity entity =new UrlEncodedFormEntity(formparams,
                    CHARSET);
            // 创建POST请求
            HttpPost request =new HttpPost(url);
            request.setEntity(entity);
            // 发送请求
            HttpClient client = getHttpClient();
            HttpResponse response = client.execute(request);
            if(response.getStatusLine().getStatusCode() != HttpStatus.SC_OK) {
                thrownew RuntimeException("请求失败");
            }
            HttpEntity resEntity =  response.getEntity();
            return (resEntity ==null) ?null : EntityUtils.toString(resEntity, CHARSET);
        } catch (UnsupportedEncodingException e) {
            Log.w(TAG, e.getMessage());
            returnnull;
        } catch (ClientProtocolException e) {
            Log.w(TAG, e.getMessage());
            returnnull;
        } catch (IOException e) {
           thrownew RuntimeException("连接失败", e);
        }
    }　
```

使用我们的CustomerHttpClient工具类

现在，在整个项目中我们都能很方便的使用该工具类来进行网络通信的业务代码编写了。下面的代码演示了如何使用username和password注册一个账户并得到新账户ID。


```
final String url ="http://yourdomain/context/adduser";    //准备数据    NameValuePair param1 =new BasicNameValuePair("username", "张三");    NameValuePair param2 =new BasicNameValuePair("password", "123456");
    int resultId =-1;    try {
        // 使用工具类直接发出POST请求,服务器返回json数据，比如"{userid:12}"
        String response = CustomerHttpClient.post(url, param1, param2);
        JSONObject root =new JSONObject(response);
        resultId = Integer.parseInt(root.getString("userid"));
        Log.i(TAG, "新用户ID："+ resultId);
    } catch (RuntimeException e) {
        // 请求失败或者连接失败
        Log.w(TAG, e.getMessage());
        Toast.makeText(this, e.getMessage(), Toast.LENGTH_SHORT);
    } catch (Exception e) {
        // JSon解析出错
        Log.w(TAG, e.getMessage());
   }
```

## 结语

可以看到，使用工具类能大大提高在项目中编写网络通信代码的效率。不过该工具类还有待完善，欢迎各位补充和矫正错误，希望最后能完成一个工具类作为使用HttpClient的最佳实践。