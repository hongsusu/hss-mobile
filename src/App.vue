<!--<template>
  <div id="app">
    <router-view/>
  </div>
</template>
<script>
  //一 网易做法
  //原理:
  // 假设我css里写了一个 div {width:1rem}
  // :
  // 这个时候如果我把html的font-size从100px修改为50px
  // 那个这个div的宽度是不是会缩小一倍?
  /**
   * 根据切图时参考的设计稿宽度, 针对对应分辨率的设备, 自动调整页面 rem 的基准值
   */
  (function () {
    var currentRootFontSize = getComputedStyle(document.documentElement, null).getPropertyValue("font-size").replace('px', '');

    function _autoAdjustRootEm() {
      // 切图时参考的设计稿宽度如750,640等
      var originalWidth = 750;
       document.documentElement.style.fontSize = document.documentElement.clientWidth / originalWidth * currentRootFontSize + 'px';
      //或者document.documentElement.style.fontSize = document.documentElement.clientWidth /(750/100)+ 'px';
    }

    // 页面加载完成前, 预调整 root em，网易做了超过1280可以设置跳转pc端或者不缩放，淘宝做了大于1080可以设置跳转pc端或者不缩放
    if (document.documentElement.clientWidth > 180) {
      _autoAdjustRootEm();
    }

    //注册 resize 事件
    window.addEventListener('resize', _autoAdjustRootEm, false);
  })();
</script>-->
<!--淘宝-->
<template>
  <div id="app">
    <router-view/>
  </div>
</template>
<script>

</script>
