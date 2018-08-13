<template>
  <div class="hello">
    简单
    <div class="test">
     便于理解的移动端适配
    </div>
  </div>
</template>
<style  lang="scss">
  @import "../less/taobaofun";
 /* 一种*/
  .hello{
   /* background: #f00;
    width: pxToRem(700);
    margin: auto;
    @include font(20px);
    .test {
      width: pxToRem(350);
      height: pxToRem(75);
      background: #0f0;
    }*/
    /*二种*/
     background: #f00;
    @include px2rem('width',700px);
    margin: auto;
    /*@include font(20px);*/
    .test {
      @include px2rem('width',350px);
      @include px2rem('height',75px);
      background: #0f0;
    }
  }
</style>
