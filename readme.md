# 《winter带你手把手实现ToyReact框架》

这是极客时间上报名的一个小课程，看winter大大从一个特别的角度切入，去了解react的内部原理，非常不错。

# 课程代码

[点击这里或者/project文件夹](./project)

# 要点笔记

### Lesson2:(41min:3s处) Range API的一个容易产生BUG的地方

> 在lesson2中，完成的代码会出现一个奇怪的bug，当从左向右点击棋盘的时候，右侧会丢失DOM节点

- 问题原因:

下面`Component`类的代码，`rerender`方法会先清空Range,而一个空Range会被相邻的Range吞掉，导致后续的内容append到了隔壁Range

``` javascript
 //Component Class
 rerender(){
    this._range.deleteContents(); //这里会清空range
    this[RENDER_TO_DOM](this._range);
  }

```
- 问题解决:

使用下面这段代码来解决这个bug:

``` javascript
 //Component Class

  let oldRange = this._range; // 保存老的range

  let range = document.createRange(); //创建新的range,放在老range的start位置
  range.setStart(oldRange.startContainer,oldRange.startOffset);
  range.setEnd(oldRange.startContainer,oldRange.startOffset);
  this[RENDER_TO_DOM](range);

  oldRange.setStart(range.endContainer,range.endOffset);
  oldRange.deleteContents();
  

```
