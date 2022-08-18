# html2pdf实践

## 需求

生成PDF文件，方案是前端用html实现基本样式，由后端使用开源库将html转为pdf, 此处主要探讨前端部分。html2pdf开源库如：iText(Java), xhtml2pdf(python), prince

## 方案实践记录

前端用静态html方式写一份验房报告。

### 页面宽高：如何使内容占一页？

A4页面大小 (210*297)mm，打印dpi: 96。换算相当于 (794*1123)px，含页边距。

页边距 5mm 相当于约 20px, 模拟页边距：(80px 60px), 相当于(20mm 15mm)

故此页面样式设置如下：

```css
html {
  background: #efefef;
}
body {
	width: 794px;
  margin: auto;
  background: #fff;
  box-sizing: border-box;
  font-size: 14px;
}
.page {
	min-height: 1123px;
  padding: 80px 60px;
  box-sizing: border-box;
}
```

这样即可实现不满一页长度的内容至少占一页高度。但对于1<x<n页的，接在后面的内容，还需要知道怎么控制其另起一页。

### 分页：如何另起一页？

有了这个属性，使内容撑满一页这个设置，在分页上就显得多余了。

```css
.page-start {
	page-break-before: always;
}
```

### 打印样式如何控制页眉页脚？

暂未实践

### 图表

canvas或svg, 是否可直接转为pdf, 即html2pdf是否支持canvas, svg，这个问题，照样使用Chrome打印来验证。

验证结果：Chrome可正常打印canvas

图表实现后转换为图片？

可将图表转成图片，再插入到报告中。

若要转换为图片，本质上应该都是将canvas转为图片，调用toDataURL，但html2pdf支持canvas标签，那转换为图片似乎就没有必要了。

但这也并不阻碍选择哪个图表库来绘图，看了下G2Plot和ECharts5, 没有直接暴露转换为图片的API。

### 检验打印效果

本地通过Chrome浏览器的打印功能来检验打印效果，Command+P键调起。

默认页边距：接近于(20mm 15mm)

设置背景颜色的，打印预览若无效，修改打印设置（勾选显示“背景图片”）即可，如表格的表头。

当文件中含打印样式时，打印设置与不含打印样式略有区别。打印样式媒体查询 @media print {}

本地装个[http-server](https://github.com/http-party/http-server), 用其启动index.html，再配合Chrome打印查看转换效果。至于实际转换效果，就看开源库的能力了。

```bash
npx http-server ./
```

## 参考资源

- [html A4打印尺寸设置](https://icode.best/i/01425742130811)
- [打印样式](https://segmentfault.com/a/1190000010145260)
