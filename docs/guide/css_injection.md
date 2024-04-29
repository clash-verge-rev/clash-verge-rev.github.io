## CSS 注入

> 将自定义的 CSS 代码注入到文档中，实现自定义样式的效果。

下列示例演示了修改部分字体、背景颜色、字体大小（层别样式没有生效可以使用 `!important`，或者无条件加上）。

<!-- prettier-ignore -->
!!! info
    本教程要求有一定的 `CSS` 代码基础，可参考[CSS文档](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/First_steps)。如果图片太小看不清，请右键图片在新标签页中打开。

![CSS 注入](../assets/guide/css_injection/css_injection.gif)

## CSS 示例代码

```css
span {
    font-family: '幼圆' !important;
}
header p {
    background-color: red !important;
    font-size: 58px !important;
}
```
