hss-mobile
===========
便于理解淘宝移动端适配方案与网易移动端适配方案的简易demo

## 一 网易的方案

- （1）设置viewport
```html
 <meta name="viewport" content="initial-scale=1,maximum-scale=1, minimum-scale=1">
``` 

- （2）动态计算html的font-size
```js
 document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px';
``` 

 **注意：**
 这里的网易方案是按照这个公式动态计算的html font-size
 ```js
   document.documentElement.style.fontSize = document.documentElement.clientWidth / originalWidth * currentRootFontSize + 'px';
 ``` 

   其结果是一样的但是这个方法需要(在动态设置前给html设置一个和$GlobalRootEmSize1一样的html的font-size，因为currentRootFontSize需要和设计稿初始html大小一致,其实除以多少都无所谓，只要别太大就可以)
- （3）布局的时候，各元素的css尺寸=设计稿标注尺寸/设计稿横向分辨率/10
- （4）font-size可能需要额外的媒介查询，并且font-size不使用rem。

```scss
$GlobalRootEmSize1: 100px;
 //这里的基准值是和动态计算的html的font-size大小要保持一致的，因为1rem=html的font-size
@function pxToRem($size, $rem: $GlobalRootEmSize1) {
  $remSize: $size / $rem;
  @return #{$remSize}rem;
}
.container {
   height:pxToRem(750px);
}

//scss翻译结果：
.container {
    height: 7.5rem;
}
```

## 二 淘宝的方案

- （1）动态设置viewport的scale
```js
var scale = 1 / devicePixelRatio;
document.querySelector('meta[name="viewport"]').setAttribute('content','initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');
```

**注意**
这个device-width的计算公式为：
设备的物理分辨率/(devicePixelRatio * scale)，在scale为1的情况下，device-width = 设备的物理分辨率/devicePixelRatio
devicePixelRatio称为设备像素比，每款设备的devicePixelRatio都是已知，并且不变的，目前高清屏，普遍都是2，不过还有更高的，比如2.5, 3 等，我魅族note的手机的devicePixelRatio就是3。淘宝触屏版布局的前提就是viewport的scale根据devicePixelRatio动态设置
- （2）动态计算html的font-size
```js
document.documentElement.style.fontSize = document.documentElement.clientWidth / 10 + 'px';
```

- （3）布局的时候，各元素的css尺寸=设计稿标注尺寸/设计稿横向分辨率/10
- （4）font-size可能需要额外的媒介查询，并且font-size不使用rem，这一点跟网易是一样的。

//less 定义一个变量和一个mixin
```less
@baseFontSize: 75;//基于视觉稿横屏尺寸/10得出的基准font-size
 //这里的基准值是和动态计算的html的font-size大小要保持一致的，因为1rem=html的font-size
.px2rem(@name, @px){
    @{name}: @px / @baseFontSize * 1rem;
}

//使用示例：
.container {
    .px2rem(height, 750);
}

//less翻译结果：
.container {
    height: 10rem;
}
```

关于这种做法的具体实现，淘宝已经给我们提供了一个开源的解决方案，具体请查看：
[参考淘宝移动端适配方案](https://github.com/amfe/lib-flexible)


## 比较网易与淘宝的做法

- 共同点：

都能适配所有的手机设备，对于pad，网易与淘宝都会跳转到pc页面，不再使用触屏版的页面
都需要动态设置html的font-size
布局时各元素的尺寸值都是根据设计稿标注的尺寸计算出来，由于html的font-size是动态调整的，所以能够做到不同分辨率下页面布局呈现等比变化
容器元素的font-size都不用rem，需要额外地对font-size做媒介查询
都能应用于尺寸不同的设计稿，只要按以上总结的方法去用就可以了

- 不同点

淘宝的设计稿是基于750的横向分辨率，网易的设计稿是基于640的横向分辨率，还要强调的是，虽然设计稿不同，但是最终的结果是一致的，设计稿的尺寸一个公司设计人员的工作标准，每个公司不一样而已
淘宝还需要动态设置viewport的scale，网易不用
最重要的区别就是：网易的做法，rem值很好计算，淘宝的做法肯定得用计算器才能用好了 。不过要是你使用了less和sass这样的css处理器，就好办多了
