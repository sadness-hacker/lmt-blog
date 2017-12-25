
# Java实现Markdown标签解析工具类

> 利用Java实现将markdown格式内容解析为html内容，基于Atlassian的CommonMark解析器实现。

> CommonMark源码: [https://github.com/akfish/CommonMark](https://github.com/akfish/CommonMark "CommonMark源码地址") 

> CommonMark简介: [https://www.oschina.net/p/commonmark-java](https://www.oschina.net/p/commonmark-java "CommonMark 简介") 

#### 1.commonmark-java 简介
commonmark-java 是一个 Markdown 解析器，一个基于 CommonMark 规范解析和渲染 Markdown 文本的 Java 库。具有以下特性：

*   小（最小化的依赖）
*   快（比 pegdown 快 10-20 倍，在仓库中可查看 benchmarks）
*	灵活（解析后可操作 AST，自定义 HTML 渲染）
*	可扩展（表格，删除线，自动链接等等）


#### 2. maven依赖,commonmark jar包

``` xml
<!-- markdown parser -->
<dependency>
    <groupId>com.atlassian.commonmark</groupId>
    <artifactId>commonmark</artifactId>
    <version>0.10.0</version>
</dependency>
```

#### 3. Markdown工具类实现

``` java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

import org.commonmark.node.Node;
import org.commonmark.parser.Parser;
import org.commonmark.renderer.html.HtmlRenderer;

/**
 * @description: markdown parser工具类
 * @author bazhandao
 * @date 2017年12月22日 下午3:59:13
 * @since JDK 1.8
 */
public class MarkdownUtil {
	
	/**
	 * Parser,线程安全
	 */
	private static Parser parser;
	
	/**
	 * HtmlRenderer,线程安全
	 */
	private static HtmlRenderer renderer;
	
	/**
	 * 初始化parser,render
	 */
	static {
		parser = Parser.builder().build();
		renderer = HtmlRenderer.builder().build();
	}
	
	/**
	 * 
	 * md2Html:markdown转html
	 * @return
	 * @author bazhandao
	 * @date 2017年12月22日 下午4:01:34
	 */
	public static String md2Html(String mdContent) {
		Node node = parser.parse(mdContent);
		String html = renderer.render(node);
		return html;
	}
	
	/**
	 * 
	 * parse:根据文件名读取md文件解析为html字符串
	 * @param filename
	 * @return
	 * @author bazhandao
	 * @date 2017年12月22日 下午4:11:02
	 */
	public static String parse(String filename) {
		StringBuilder sb = new StringBuilder();
		try {
			Files.readAllLines(Paths.get("filename")).forEach(line -> {
				sb.append(line);
			});
		} catch (IOException e) {
			e.printStackTrace();
		}
		return md2Html(sb.toString());
	}
}
```
