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

### 走起来！

这里使用es6的```class```来给出一种实现   

对于测试用例中直接写在```<script>```标签中的这种形式也势必会面对浏览器的兼容问题   

但既然题设里没有限制，就暂且先这样写啦

```
class DragElement {
  constructor(target) {
    if (!target) {
      throw new Error('can not init without an element!')
    }
    this.target = target;
    this.eleLeft = this._getStyle('left');
    this.eleTop = this._getStyle('top');
    this.targetHeight = parseInt(this._getStyle('height'));
    this.targetWidth = parseInt(this._getStyle('width'));
    this.currentX = 0;
    this.currentY = 0;
    this.isDraging = false;

    target.addEventListener('mousedown', (event) => {
      const e = this._handleEvent(event)
      this.isDraging = true;
      this.currentX = e.clientX;
      this.currentY = e.clientY;
    });

    document.addEventListener('mouseup', () => {
      this.isDraging = false;
      this.eleLeft = this._getStyle('left');
      this.eleTop = this._getStyle('top');
    })

    document.addEventListener('mousemove', (e) => {
      if (this.isDraging) {
        const e = this._handleEvent(event)
        const clientX = this._getDis(e.clientX, document.body.clientWidth, this.targetWidth);
        const clientY = this._getDis(e.clientY, document.body.clientHeight, this.targetHeight);
        const moveX = clientX - this.currentX
        const moveY = clientY - this.currentY;
        this.target.style.left = parseInt(this.eleLeft) + moveX + "px";
        this.target.style.top = parseInt(this.eleTop) + moveY + "px";
        return false;
      }
    })
  }

  _handleEvent(event) {
    return event || window.event;
  }

  _getStyle(style) {
    return this.target.currentStyle ? this.target.currentStyle[style] : window.getComputedStyle(this.target, false)[style];
  }

  _getDis(minDis, maxDis, targetAttr) {
    return Math.min(Math.max(minDis, targetAttr), maxDis - targetAttr);
  }
}

new DragElement(document.querySelector('#container'))

```