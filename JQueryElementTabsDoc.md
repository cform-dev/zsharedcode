﻿#summary JQueryElement 分组标签
#labels Phase-Implementation
<font face='microsoft yahei'>
<font size='1'><a href='http://www.microsofttranslator.com/bv.aspx?from=&to=en&a=http://code.google.com/p/zsharedcode/wiki/JQueryElementTabsDoc'>Translate this page</a></font>

<h3>简介</h3>
<blockquote>使用 JQueryElement 的 Tabs 控件即可实现 jQuery UI 中的分组标签.</blockquote>

<h3>前提条件</h3>
<ol><li>请在 <a href='Download.md'>下载资源</a> 中的 JQueryElement.dll 下载一节下载 JQueryElement 3.0 或更高版本的 dll, 并为项目引用对应 .NET 版本的 dll.</li></ol>

<blockquote>2. 在页面添加如下指令:<br>
<pre><code>&lt;%@ Register Assembly="zoyobar.shared.panzer.JQueryElement" Namespace="zoyobar.shared.panzer.ui.jqueryui" TagPrefix="je" %&gt;
</code></pre></blockquote>

<blockquote>3. JQueryElement 并没有将 jQuery UI 的脚本和样式作为资源嵌入, 所以请将 jQuery UI 所需的脚本和样式复制到项目中并在页面中引用, 比如:<br>
<pre><code>&lt;script type="text/javascript" src="../js/jquery-1.5.1.min.js"&gt;&lt;/script&gt;
&lt;script type="text/javascript" src="../js/jquery-ui-1.8.11.custom.min.js"&gt;&lt;/script&gt;
&lt;link type="text/css" rel="Stylesheet" href="../css/smoothness/jquery-ui-1.8.15.custom.css" /&gt;
</code></pre></blockquote>

<blockquote>4. 添加如下 js 脚本:<br>
<pre><code>&lt;script type="text/javascript"&gt;
	function writeLine(selector, html) {
		$(selector).html($(selector).html() + html + '&lt;br /&gt;');
	}
&lt;/script&gt;
</code></pre></blockquote>

<blockquote>5. 页面包含如下自定义样式, 请参考文章尾部的 main.css.<br>
<pre><code>&lt;link type="text/css" rel="Stylesheet" href="../css/main.css" /&gt;
</code></pre></blockquote>

<h3>添加 ScriptPackage 控件</h3>
<blockquote>添加 ScriptPackage 控件, 用来统一存放控件产生的 js 脚本, 也可以不添加. 需要将控件放到页面代码的尾部, 否则有些 js 脚本可能不会被包含.<br>
<pre><code>&lt;je:ScriptPackage ID="package" runat="server" /&gt;
</code></pre></blockquote>

<h3>通过选择器实现分组标签</h3>
<blockquote>Tabs 可以使页面上其它的元素实现分组标签, 通过 Selector 属性, 将其设置为一个选择器, 比如: <code>'#dA'</code>, 表示选择页面中 id 为 dA 的元素, 注意使用了单引号, 那么 dA 将成为一个分组标签, 其对应的 js 脚本将生成到刚才添加的 ScriptPackage 控件中.<br>
<pre><code>&lt;div id="dA" style="width: 500px;"&gt;
	&lt;ul&gt;
		&lt;li&gt;&lt;a href="#tab11"&gt;欢迎&lt;/a&gt;&lt;/li&gt;
		&lt;li&gt;&lt;a href="#tab13"&gt;联系方式&lt;/a&gt;&lt;/li&gt;
	&lt;/ul&gt;
	&lt;div id="tab11"&gt;
		&lt;strong&gt;welcome&lt;/strong&gt;, 这是 Tabs.
	&lt;/div&gt;
	&lt;div id="tab13"&gt;
		&lt;strong&gt;邮箱:&lt;/strong&gt; ...
	&lt;/div&gt;
&lt;/div&gt;
&lt;je:Tabs ID="tA" runat="server" ScriptPackageID="package" Selector="'#dA'"&gt;
&lt;/je:Tabs&gt;
</code></pre></blockquote>

<h3>效果说明</h3>
<blockquote>通过设置 Tabs 的属性实现的部分效果如下, 具体请参考 tabs.aspx:<br>
</blockquote><ul><li>缓存载入的页面<br>
</li><li>再次点击激活的标签可以折叠<br>
</li><li>在为多个元素设置拖动效果时, 取消其中部分元素的拖动效果<br>
</li><li>设置激活的标签</li></ul>

<h3>事件说明</h3>
<blockquote>Tabs 控件具有如下事件, 具体请参考 tabs.aspx:<br>
</blockquote><ul><li>创建时<br>
</li><li>显示某个标签时</li></ul>

<h3>tabs.aspx</h3>
<pre><code>&lt;%@ Page Language="C#" %&gt;

&lt;%@ Register Assembly="zoyobar.shared.panzer.JQueryElement" Namespace="zoyobar.shared.panzer.ui.jqueryui"
	TagPrefix="je" %&gt;
&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;
&lt;script runat="server"&gt;

	protected void tF_SelectSync ( object sender, TabsEventArgs e )
	{
		this.ClientScript.RegisterStartupScript ( this.GetType ( ), "tF_SelectSync", "alert('tF_SelectSync');", true );
	}
	
&lt;/script&gt;
&lt;html xmlns="http://www.w3.org/1999/xhtml"&gt;
&lt;head runat="server"&gt;
	&lt;title&gt;JQuery UI 的 tabs&lt;/title&gt;
	&lt;script type="text/javascript" src="../js/jquery-1.5.1.min.js"&gt;&lt;/script&gt;
	&lt;script type="text/javascript" src="../js/jquery-ui-1.8.11.custom.min.js"&gt;&lt;/script&gt;
	&lt;link type="text/css" rel="Stylesheet" href="../css/smoothness/jquery-ui-1.8.15.custom.css" /&gt;
	&lt;link type="text/css" rel="Stylesheet" href="../css/main.css" /&gt;
	&lt;script type="text/javascript"&gt;
		function writeLine(selector, html) {
			$(selector).html($(selector).html() + html + '&lt;br /&gt;');
		}
	&lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;
	&lt;form id="formTabs" runat="server"&gt;
	&lt;div id="dA" style="width: 500px;"&gt;
		&lt;ul&gt;
			&lt;li&gt;&lt;a href="#tab11"&gt;欢迎&lt;/a&gt;&lt;/li&gt;
			&lt;li&gt;&lt;a href="#tab13"&gt;联系方式&lt;/a&gt;&lt;/li&gt;
		&lt;/ul&gt;
		&lt;div id="tab11"&gt;
			&lt;strong&gt;welcome&lt;/strong&gt;, 这是 Tabs.
		&lt;/div&gt;
		&lt;div id="tab13"&gt;
			&lt;strong&gt;邮箱:&lt;/strong&gt; ...
		&lt;/div&gt;
	&lt;/div&gt;
	&lt;je:Tabs ID="tA" runat="server" ScriptPackageID="package" Selector="'#dA'"&gt;
	&lt;/je:Tabs&gt;
	&lt;br /&gt;
	&lt;span class="code"&gt;Selector="'#dA'"&lt;/span&gt;
	&lt;br /&gt;
	&lt;hr /&gt;
	&lt;div id="dB" style="width: 500px;"&gt;
		&lt;ul&gt;
			&lt;li&gt;&lt;a href="#tab21"&gt;欢迎&lt;/a&gt;&lt;/li&gt;
			&lt;li&gt;&lt;a href="base.htm"&gt;基本信息&lt;/a&gt;&lt;/li&gt;
			&lt;li&gt;&lt;a href="#tab23"&gt;联系方式&lt;/a&gt;&lt;/li&gt;
		&lt;/ul&gt;
		&lt;div id="tab21"&gt;
			&lt;strong&gt;welcome&lt;/strong&gt;, 这是 Tabs, 缓存 "基本信息" 的 base.htm 页面.
		&lt;/div&gt;
		&lt;div id="tab23"&gt;
			&lt;strong&gt;邮箱:&lt;/strong&gt; ...
		&lt;/div&gt;
	&lt;/div&gt;
	&lt;je:Tabs ID="tB" runat="server" ScriptPackageID="package" Selector="'#dB'" Cache="True"&gt;
	&lt;/je:Tabs&gt;
	&lt;br /&gt;
	&lt;span class="code"&gt;Cache="True"&lt;/span&gt;
	&lt;br /&gt;
	&lt;hr /&gt;
	&lt;div id="dC" style="width: 500px;"&gt;
		&lt;ul&gt;
			&lt;li&gt;&lt;a href="#tab31"&gt;欢迎&lt;/a&gt;&lt;/li&gt;
			&lt;li&gt;&lt;a href="#tab33"&gt;联系方式&lt;/a&gt;&lt;/li&gt;
		&lt;/ul&gt;
		&lt;div id="tab31"&gt;
			&lt;strong&gt;welcome&lt;/strong&gt;, 这是 Tabs, 允许折叠展开的内容.
		&lt;/div&gt;
		&lt;div id="tab33"&gt;
			&lt;strong&gt;邮箱:&lt;/strong&gt; ...
		&lt;/div&gt;
	&lt;/div&gt;
	&lt;je:Tabs ID="tC" runat="server" ScriptPackageID="package" Selector="'#dC'" Collapsible="True"&gt;
	&lt;/je:Tabs&gt;
	&lt;br /&gt;
	&lt;span class="code"&gt;Collapsible="True"&lt;/span&gt;
	&lt;br /&gt;
	&lt;hr /&gt;
	&lt;div id="dD" style="width: 500px;"&gt;
		&lt;ul&gt;
			&lt;li&gt;&lt;a href="#tab41"&gt;欢迎&lt;/a&gt;&lt;/li&gt;
			&lt;li&gt;&lt;a href="#tab43"&gt;联系方式&lt;/a&gt;&lt;/li&gt;
		&lt;/ul&gt;
		&lt;div id="tab41"&gt;
			&lt;strong&gt;welcome&lt;/strong&gt;, 这是 Tabs.
		&lt;/div&gt;
		&lt;div id="tab43"&gt;
			&lt;strong&gt;邮箱:&lt;/strong&gt; ..., 默认选中.
		&lt;/div&gt;
	&lt;/div&gt;
	&lt;je:Tabs ID="tD" runat="server" ScriptPackageID="package" Selector="'#dD'" Selected="1"&gt;
	&lt;/je:Tabs&gt;
	&lt;br /&gt;
	&lt;span class="code"&gt;Selected="1"&lt;/span&gt;
	&lt;br /&gt;
	&lt;hr /&gt;
	&lt;div id="dE" style="width: 500px;"&gt;
		&lt;ul&gt;
			&lt;li&gt;&lt;a href="#tab51"&gt;欢迎&lt;/a&gt;&lt;/li&gt;
			&lt;li&gt;&lt;a href="#tab53"&gt;联系方式&lt;/a&gt;&lt;/li&gt;
		&lt;/ul&gt;
		&lt;div id="tab51"&gt;
			&lt;strong&gt;welcome&lt;/strong&gt;, 这是 Tabs, 事件.
		&lt;/div&gt;
		&lt;div id="tab53"&gt;
			&lt;strong&gt;邮箱:&lt;/strong&gt; ...
		&lt;/div&gt;
	&lt;/div&gt;
	&lt;je:Tabs ID="tE" runat="server" ScriptPackageID="package" Selector="'#dE'" Create="function(){writeLine('#pE', '创建');}"
		Select="function(){writeLine('#pE', '选择');}" Show="function(){writeLine('#pE', '显示');}"&gt;
	&lt;/je:Tabs&gt;
	&lt;p id="pE" class="panel" style="width: 80%;"&gt;
	&lt;/p&gt;
	&lt;br /&gt;
	&lt;span class="code"&gt;Create="..." Select="..." Show="..."&lt;/span&gt;
	&lt;br /&gt;
	&lt;hr /&gt;
	&lt;div id="dF" style="width: 500px;"&gt;
		&lt;ul&gt;
			&lt;li&gt;&lt;a href="#tab61"&gt;欢迎&lt;/a&gt;&lt;/li&gt;
			&lt;li&gt;&lt;a href="#tab63"&gt;联系方式&lt;/a&gt;&lt;/li&gt;
		&lt;/ul&gt;
		&lt;div id="tab61"&gt;
			&lt;strong&gt;welcome&lt;/strong&gt;, 这是 Tabs, 服务器端事件.
		&lt;/div&gt;
		&lt;div id="tab63"&gt;
			&lt;strong&gt;邮箱:&lt;/strong&gt; ...
		&lt;/div&gt;
	&lt;/div&gt;
	&lt;je:Tabs ID="tF" runat="server" ScriptPackageID="package" Selector="'#dF'" 
		onselectsync="tF_SelectSync"&gt;
	&lt;/je:Tabs&gt;
	&lt;br /&gt;
	&lt;span class="code"&gt;onselectsync="tF_SelectSync"&lt;/span&gt;
	&lt;br /&gt;
	&lt;hr /&gt;
	&lt;je:ScriptPackage ID="package" runat="server" /&gt;
	&lt;/form&gt;
&lt;/body&gt;
&lt;/html&gt;
</code></pre>

<h3>base.htm</h3>
<pre><code>&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;
&lt;html xmlns="http://www.w3.org/1999/xhtml"&gt;
&lt;head&gt;
	&lt;title&gt;基本信息&lt;/title&gt;
&lt;/head&gt;
&lt;body&gt;
	页面 base.htm 里显示您的基本信息, ...
&lt;/body&gt;
&lt;/html&gt;
</code></pre>

<h3>main.css</h3>
<pre><code>body
{
	font-family: "微软雅黑";
	font-size: 9pt;
}
hr
{
	border: solid 1px #eeeeee;
	margin-bottom: 50px;
}
li
{
	padding: 5px;
}
.panel
{
	border: solid 1px #cccccc;
	padding: 10px;
	background-color: #eeeeee;
}
.box
{
	border: solid 1px #999999;
	padding: 2px 5px 2px 5px;
	color: InfoText;
	background-color: InfoBackground;
}
.code
{
	float: right;
	font-style: italic;
	font-size: x-small;
	color: Blue;
}
.ui-selecting
{
	color: MenuText;
	background-color: InactiveCaption;
}
.ui-selected
{
	color: MenuText;
	background-color: ActiveCaption;
}
</code></pre>

<blockquote><i>这里仅列出部分代码, 可能需要您自己创建窗口, 页面等.</i>
</font>