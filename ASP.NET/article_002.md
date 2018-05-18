# 压缩aspx页面，移除aspx多余的空格
可以将动态的aspx页面进行压缩，只需要继承就可以。
```java
using System;
using System.IO;
using System.Web.UI;
using System.Text.RegularExpressions;
/// <summary>
///PageBase 的摘要说明
/// </summary>public class PageBase : System.Web.UI.Page
{
	protected override void Render(HtmlTextWriter writer)
	{

		StringWriter sw = new StringWriter();

		HtmlTextWriter htmlWriter = new HtmlTextWriter(sw);
		base.Render(htmlWriter);
		string html = sw.ToString();

		html = Regex.Replace(html, "[\f\n\r\t\v]", "");

		html = Regex.Replace(html, " {2,}", " ");

		html = Regex.Replace(html, ">[ ]{1}", ">");


		writer.Write(html);

	}

}
```