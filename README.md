# html2pdf实践

## 需求

生成PDF文件，方案是前端用html实现基本样式，由后端使用开源库将html转为pdf, 此处主要探讨前端部分。html2pdf开源库如：iText(Java), xhtml2pdf(python), prince

## 方案实践记录

前端用静态html方式写一份验房报告。

### 页面宽高：如何使内容占一页？

A4页面大小 (210*297)mm，打印dpi: 96。换算相当于 (794*1123)px，含页边距。

页边距 5mm 相当于约 20px, 模拟页边距：(80px 60px), 相当于(20mm 15mm)

但页边距正确方式由打印样式来控制，无需在页面内容中模拟，页面主体内容将被打印在页面内容区域，只需设置网页主体宽度，使得与打印后预览效果接近即可。

打印后常规字体大小18px，效果较为合适。

故此页面样式设置如下：

```css
/* 打印样式 */
@media print {
  html, body {
    height: 297mm;
    box-sizing: border-box;
  }
}
@page {
  size: A4 portrait;
  margin: 20mm 15mm;
}

html {
  background: #fff;
}
body {
	width: 794px;
  margin: auto;
  background: #fff;
  box-sizing: border-box;
  font-size: 18px;
}
/* .page {
	min-height: 1123px;
  padding: 80px 60px;
  box-sizing: border-box;
} */
```

### 分页：如何另起一页？

属性 | 可选值 | 含义
---- | ---- | ----
page-break-before | auto、always、avoid、left、right、recto、verso | 开头重起一页
page-break-after | auto、always、avoid、left、right、recto、verso | 结尾换页
page-break-inside | avoid, auto | 该元素避免分页，保持在同一页

```css
.page-start {
	page-break-before: always;
}
```

### 打印样式如何控制页眉页脚？

暂未实践

### 图表：是否支持canvas/svg

svg标签跟其他html标签无异，可正常打印

canvas标签，需支持javascript代码执行，只要脚本正常执行，也能打印

图表多是使用第三方库绘制的，是否支持第三方库？

图表实现后转换为图片？

可将图表转成图片，再插入到报告中。

若要转换为图片，本质上应该都是将canvas转为图片，调用toDataURL，但html2pdf支持canvas标签，那转换为图片似乎就没有必要了。

### 检验打印效果-Chrome浏览器自带

本地通过Chrome浏览器的打印功能来检验打印效果，Command+P键调起。

默认页边距：接近于(20mm 15mm)

设置背景颜色的，打印预览若无效，修改打印设置（勾选显示“背景图片”）即可，如表格的表头。

当文件中含打印样式时，打印设置与不含打印样式略有区别。打印样式媒体查询 @media print {}

本地装个[http-server](https://github.com/http-party/http-server), 用其启动index.html，再配合Chrome打印查看转换效果。至于实际转换效果，就看开源库的能力了。

```bash
npx http-server ./
```

### 检验打印效果-CutyCapt

一个跨平台的命令行工具，可将网页转化为pdf、图片等格式

## 参考资源

- [html A4打印尺寸设置](https://icode.best/i/01425742130811)
- [打印样式](https://segmentfault.com/a/1190000010145260)
- [CutyCapt ubuntu](https://manpages.ubuntu.com/manpages/jammy/man1/cutycapt.1.html)
- [CutyCapt ubuntu](http://cutycapt.sourceforge.net/)
