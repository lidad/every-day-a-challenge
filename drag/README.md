## 原生JS实现 DOM 拖动效果

使用原声js实现DOM拖动   

注意：  
- 事件兼容
- 拖拽边界处理，比如浏览器边界，限制在某个容器内拖动，限制在X轴或Y轴方向拖动等，可以只选择某一种方案来实现
- 拖拽体验，比如拖动连贯性，拖动过程中鼠标位置是否正确等，可以在浏览器中调试   

用例如下：

```
<div id="container" style="border:1px solid red; position: absolute; width:100px; height: 100px">something</div>

<script>
    // code here
</script>
```