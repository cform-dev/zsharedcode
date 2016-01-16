﻿#summary Timer 完全参考
#labels Phase-Implementation
<font face='microsoft yahei'>
<font size='1'><a href='http://www.microsofttranslator.com/bv.aspx?from=&to=en&a=http://code.google.com/p/zsharedcode/wiki/JQueryElementTimerDoc'>Translate this page</a></font>

<h3>简介/目录</h3>
<blockquote>请到 <a href='Download.md'>下载资源</a> 的 JQueryElement 示例下载一节*下载示例代码<b>, 目录 /timer/Default.aspx.</blockquote></b>

<blockquote>视频演示: <a href='http://www.tudou.com/programs/view/bGiJ5XUyfVI/'>www.tudou.com/programs/view/bGiJ5XUyfVI/</a></blockquote>

<blockquote>本文将说明 Timer 控件的功能以及使用过程中的注意事项和技巧, 目录如下:</blockquote>

<ul><li>准备<br>
</li><li>触发间隔<br>
</li><li>客户端触发事件<br>
</li><li>服务器端触发事件<br>
</li><li>启动和停止时钟<br>
</li><li>附录: 新邮件提醒示例分析</li></ul>

<img src='http://zsharedcode.googlecode.com/files/newemaillist.jpg' />

<h3>准备</h3>
<blockquote>请确保已经在 <a href='Download.md'>下载资源</a> 中的 JQueryElement.dll 下载一节下载 JQueryElement 最新的版本.</blockquote>

<blockquote>请使用指令引用如下的命名空间:<br>
<pre><code>&lt;%@ Register Assembly="zoyobar.shared.panzer.JQueryElement"
	Namespace="zoyobar.shared.panzer.ui.jqueryui.plusin"
	TagPrefix="je" %&gt;
&lt;%@ Register Assembly="zoyobar.shared.panzer.JQueryElement"
	Namespace="zoyobar.shared.panzer.web.jqueryui"
	TagPrefix="je" %&gt;
</code></pre>
除了命名空间, 还需要引用 jQueryUI 的脚本和样式, 在 <a href='Download.md'>下载资源</a> 的 JQueryElement.dll 下载一节下载的压缩包中包含了一个自定义样式的 jQueryUI, 如果需要更多样式, 可以在 <a href='http://jqueryui.com/download'>jqueryui.com/download</a> 下载:<br>
<pre><code>&lt;link type="text/css" rel="stylesheet" href="[样式路径]/jquery-ui-&lt;version&gt;.custom.css" /&gt;
&lt;script type="text/javascript" src="[脚本路径]/jquery-&lt;version&gt;.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="[脚本路径]/jquery-ui-&lt;version&gt;.custom.min.js"&gt;&lt;/script&gt;
</code></pre>
也可以使用 ResourceLoader 来添加所需的脚本或样式, 详细请参考 <a href='ResourceLoader.md'>自动添加脚本和样式</a>.</blockquote>

<h3>触发间隔</h3>
<blockquote>可以通过 Interval 属性来设置时钟的触发时间间隔, 以毫秒为单位, 默认为 1000 毫秒.</blockquote>

<h3>客户端触发事件</h3>
<blockquote>Timer 的 Tick 属性表示在客户端运行的触发事件:<br>
<pre><code>&lt;je:Timer ID="checkTimer" runat="server" Tick="
function(pe, e){
	alert('触发次数 ' + e.count.toString());
}
"&gt;
&lt;/je:Timer&gt;
</code></pre>
将 Tick 属性设置为如上形式的 javascript 函数, 即可在对应的 javascript 函数中编写触发时执行的代码. 其中, 参数 e 的 count 属性表示 <b>timer</b> 开始计时后第几次触发事件.</blockquote>

<h3>服务器端触发事件</h3>
<blockquote>设置 TickAsync 属性, 可以调用服务器端的方法, 形式如下:<br>
<pre><code>&lt;je:Timer ID="checkTimer" runat="server"&gt;
	&lt;TickAsync Url="&lt;触发事件地址&gt;" MethodName="&lt;触发事件名称&gt;" Success="&lt;处理结果的 javascript 函数&gt;"&gt;

		&lt;je:Parameter Name="&lt;参数名1&gt;"
			Type="Expression"
			Value="&lt;值1&gt;"
			Default="&lt;默认值1&gt;" /&gt;
		&lt;je:Parameter Name="&lt;参数名2&gt;"
			Type="Selector"
			Value="&lt;选择器2&gt;"
			Default="&lt;默认值2&gt;" /&gt;

	&lt;/TickAsync&gt;
&lt;/je:Timer&gt;

&lt;je:Timer ID="checkTimer" runat="server"&gt;
	&lt;TickAsync Url="webservice.asmx" MethodName="NewEMailCount" Success="
	function(data){

	}
	"&gt;
	&lt;/TickAsync&gt;
&lt;/je:Timer&gt;
</code></pre>
其中, 通过 Parameter 可以为 Ajax 调用添加参数, 更多 Parameter 的信息, 请参考 <a href='JEParameter.md'>通过 Parameter 对象添加 Ajax 请求时的参数</a>.</blockquote>

<blockquote>代码中的 Success 为处理服务器返回 JSON 的 javascript 函数, 这里采用的是 .NET 2.0 下的写法, 不同写法请参考 <a href='AjaxReturnJSON.md'>在不同的 .NET 版本中返回 JSON</a>.</blockquote>

<h3>启动和停止时钟</h3>
<blockquote>在 javascript 中, 调用 <b>timer</b> 的 start 和 stop 方法即可启动或者停止时钟, 语法为 <code>&lt;timer 变量&gt;.__timer('start'); &lt;timer 变量&gt;.__timer('stop');</code>:<br>
<pre><code>&lt;script type="text/javascript"&gt;
	$(function () {

		checkTimer.__timer('start');

	});
&lt;/script&gt;
</code></pre></blockquote>

<h3>附录: 新邮件提醒示例分析</h3>
<blockquote>这一节将说明新邮件提醒这个例子的大概设计思路, 在页面上使用了 Timer 控件来定时从 WebService 获取新邮件的有关信息:<br>
<pre><code>&lt;je:Timer ID="checkTimer" runat="server" IsVariable="true" Interval="5000"&gt;
	&lt;TickAsync Url="webservice.asmx" MethodName="NewEMailCount" Success="
	function(data){
		// 如果是 .NET 3.5, 4.0 需要换成 data.d
		newEMailCount += data;

		if(newEMailCount != 0){
			$('#newcount').text(newEMailCount.toString());
			newDialog.dialog('open');
		}

	}
	"&gt;
	&lt;/TickAsync&gt;
&lt;/je:Timer&gt;
</code></pre>
方法 NewEMailCount 将返回 5 秒内的新邮件个数, 这个数将累计到 javascript 变量 newEMailCount 中, 如果 newEMailCount 不为 0, 则显示对话框将显示新邮件的个数, 一旦对话框关闭, newEMailCount 将被设置为 0:<br>
<pre><code>&lt;je:Dialog ID="newDialog" runat="server" IsVariable="true"
	AutoOpen="false" Position="['right', 'bottom']"
	Html='您有 &lt;strong id="newcount"&gt;&lt;/strong&gt; 封新邮件'
	Buttons="{'刷新': function(){ emailRepeater.__repeater('filter'); newDialog.dialog('close'); }}"
	Close="function(){ newEMailCount = 0; }"&gt;
&lt;/je:Dialog&gt;
</code></pre>
新邮件个数显示在标签 newcount 中, 而在 Dialog 的 Close 属性中, 编写了一个 javascript 函数来设置 newEMailCount 为 0. Dialog 的 Buttons 属性定义了一个刷新按钮, 在按钮的点击事件中调用了 <b>repeater</b> 的 filter 方法, 这样可以使邮件列表刷新, 更多 Repeater 的信息可以参考 <a href='JQueryElementRepeaterDoc.md'>Repeater 完全参考</a>, 这里就不再解释了, 邮件的列表代码如下:<br>
<pre><code>&lt;table id="list"&gt;
	&lt;je:Repeater ID="emailRepeater" runat="server" IsVariable="true" Selector="#list"
		PageSize="4" FillAsync-Url="webservice.asmx" FillAsync-MethodName="GetEMailList"&gt;
		&lt;HeaderTemplate&gt;
			&lt;thead&gt;
				&lt;tr&gt;
					&lt;td&gt;
						发信人
					&lt;/td&gt;
					&lt;td&gt;
						标题
					&lt;/td&gt;
					&lt;td&gt;
						时间
					&lt;/td&gt;
				&lt;/tr&gt;
			&lt;/thead&gt;
		&lt;/HeaderTemplate&gt;
		&lt;ItemTemplate&gt;
			&lt;tr&gt;
				&lt;td class="sender"&gt;
				#{sender}
				&lt;/td&gt;
				&lt;td class="#{isnew,# ? 'new-mail' : ''}"&gt;
				#{title}
				&lt;/td&gt;
				&lt;td class="timer"&gt;
				#{time,jQuery.panzer.formatDate(#,'yyyy-M-d')}
				&lt;/td&gt;
			&lt;/tr&gt;
		&lt;/ItemTemplate&gt;
		&lt;FooterTemplate&gt;
			&lt;tfoot&gt;
				&lt;tr&gt;
					&lt;td colspan="2"&gt;
		&lt;a href="#" je-onclick="prev"&gt;上一页&lt;/a&gt;
		&lt;a href="#" je-onclick="next"&gt;下一页&lt;/a&gt;,
		第 @{pageindex}/@{pagecount} 页, 共 @{itemcount} 条,
		&lt;a href="#" je-onclick="goto,new Number(jQuery('#pageindex').val())"&gt;
		跳转
		&lt;/a&gt;
		到第
		&lt;input type="text" id="pageindex" value="@{pageindex}" /&gt;
		页.
					&lt;/td&gt;
				&lt;/tr&gt;
			&lt;/tfoot&gt;
		&lt;/FooterTemplate&gt;
	&lt;/je:Repeater&gt;
&lt;/table&gt;
</code></pre>
在页面中, 还添加了发送新邮件的文本框和按钮, 发送按钮将调用服务器端的 SendEMail 方法, 此方法将新邮件保存在 DataTable 中, 并使新邮件的个数加 1, 这样 NewEMailCount 才能返回新的邮件个数:<br>
<pre><code>&lt;strong&gt;发信人:&lt;/strong&gt;
&lt;input type="text" id="eSender" /&gt;
&lt;je:Validator ID="vSender" runat="server" IsVariable="true" Target="#eSender"
	Need="true" NeedTip='&lt;font color="red"&gt;请填写发信人&lt;/font&gt;'
	Reg="$.panzer.reg.email"
	RegTip='&lt;font color="red"&gt;请填写一个正确的邮箱地址&lt;/font&gt;'
	Checked="refreshSendButton"&gt;
&lt;/je:Validator&gt;
&lt;br /&gt;
&lt;br /&gt;
&lt;strong&gt;标题:&amp;nbsp;&amp;nbsp;&amp;nbsp;&lt;/strong&gt;
&lt;input type="text" id="eTitle" /&gt;
&lt;je:Validator ID="vTitle" runat="server" IsVariable="true" Target="#eTitle"
	Need="true" NeedTip='&lt;font color="red"&gt;请填写标题&lt;/font&gt;'
	Checked="refreshSendButton"&gt;
&lt;/je:Validator&gt;
&lt;br /&gt;
&lt;br /&gt;
&lt;je:Button ID="cmdSend" runat="server" IsVariable="true" Label="发送" Disabled="true"&gt;
	&lt;ClickAsync Url="webservice.asmx" MethodName="SendEMail" Success="
	function(data){
		alert(data);
	}
	"&gt;
		&lt;je:Parameter Name="sender" Type="Expression"
			Value="vSender.__validator('option','value')" /&gt;
		&lt;je:Parameter Name="title" Type="Selector"
			Value="#eTitle" /&gt;
	&lt;/ClickAsync&gt;
&lt;/je:Button&gt;
</code></pre>
代码中, 使用了 Validator 来验证了用户输入的发信人和标题, 这里也不解释了, 可以参考 <a href='JQueryElementValidatorDoc.md'>Validator 完全参考</a>.</blockquote>

<h3>相关内容</h3>
<blockquote><a href='AjaxReturnJSON.md'>在不同的 .NET 版本中返回 JSON</a></blockquote>

<blockquote><a href='JEBase.md'>JQueryElement 基本属性参考</a></blockquote>

<blockquote><a href='JEParameter.md'>通过 Parameter 对象添加 Ajax 请求时的参数</a></blockquote>

<blockquote><a href='JQueryElementRepeaterDoc.md'>Repeater 完全参考</a></blockquote>

<blockquote><a href='JQueryElementValidatorDoc.md'>Validator 完全参考</a></blockquote>

<blockquote><a href='ResourceLoader.md'>自动添加脚本和样式</a></blockquote>

<h3>修订历史</h3>
<blockquote>2011-11-26: 修改关于引用 jQueryUI 的介绍.</blockquote>

<blockquote>2011-12-5: 修改下载的链接.</blockquote>

<blockquote>2012-1-26: 增加关于 ResourceLoader 的链接.</blockquote>

</font>