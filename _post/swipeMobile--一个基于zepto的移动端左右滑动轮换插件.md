---
title: swipeMobile--一个基于zepto的移动端左右滑动轮换插件
subtitle: "js切换插件"
date: 2016-08-28 17:19:19
tags:
    - 移动端
    - js插件
    - zepto.js
    - 左右轮换
cdn: 'header-off'
header-img: 'http://www.codeblocq.com/assets/projects/hexo-theme-clean-blog/img/home-bg.jpg'
---

平时做移动端项目，经常会遇到左右切换轮换的功能，网上插件很多, 像比较好的[Swiper](http://www.swiper.com.cn/) 等等, 但是这个插件功能太多, 我只需要简单的轮换, 就显得插件有点庞大, 于是自己写了个很简单实现基础功能的.

<!-- more -->

源码跟demo已发上github, 地址是: https://github.com/Moonphy/swipeMobile 因为写的不好, 有能指点如何修改的先谢过哈, 要是觉得还实用的就在github上点个star咯哈哈!!

## 示例图

![示例图](http://7xoyoo.com1.z0.glb.clouddn.com/image/jpg/1.gif)

## 调用

看demo, 直接在HTML里调用就行
``` bash
<script src="../js/zepto.min.js"></script>
<script src="../js/swipeMobile.js"></script>
<script>
  // 设置自己需要的配置  
  var hotSwiper = new swipeMobile('#hot', {"size": 2, "playTime": 4000,"auto": false, "scrollTime": 800});
  // 默认模式
  var newSwiper = new swipeMobile('#new', {})
</script>
```

## 参数

``` bash
// 构造函数是这样的:
function swipeMobile(box, config){}
// 可配置的参数:
/**
 * @box 滑动的ul标签, 使用自己定义的类或id或直接标签, 你喜欢!
 * @playTime 自动滑动的间隔时长, 默认是3000ms
 * @size 每屏显示的item个数, 默认一屏显示3个/一次切换三个
 * @auto 是否自动滑动, 默认自动滑动
 * @loop 是否循环滑动, 默认可循环
 * @scrollTime 动画完成的时长, 默认是1000ms
 */
```
## 样式
样式是根据自己需要去自己写, 但下标的轮换指示点结构是固定的: 
``` bash
<div class="slide-btn">
    <span class="active"></span>
</div>
```
然后点的个数是js判断自动加减的, 写自己需要的样式就行!



## 最后, 贴个代码吧
``` bash
/**
 * Created by Moonphy on 2016/8/25.
 * @box 滑动的ul
 * @playTime 自动滑动的间隔时长
 * @size 每屏显示的item个数
 * @auto 是否自动滑动
 * @loop 是否循环滑动
 * @scrollTime 动画完成的时长
 */
;(function ($) {
  function swipeMobile(box, config){

    this.box = $(box);
    this.child = this.box.children();
    this.parent = this.box.parent();
    this.dotWrap = this.parent.find('.slide-btn');
    this.setting = {
      "playTime": 3000,
      "size": 3,
      "auto": true,
      "loop": true,
      "scrollTime": 1000
    };
    $.extend(this.setting,config);

    this.cNum = this.child.length;
    this.winWidth = 0;
    this.wNum = 1;

    this.init();
    this.events();
  }
  swipeMobile.prototype = {

    init: function () {
      this.wNum = Math.ceil(this.cNum/this.setting.size);
      this.winWidth = this.parent.width();
      this.box.width(this.winWidth * this.wNum);
      this.parent.height(this.box.height() + this.dotWrap.height());
      this.child.width(this.winWidth/this.setting.size);

      if(this.wNum>1){
        for(var i=1;i<this.wNum;i++){
          this.dotWrap.append('<span>')
        }
      }

    },
    events: function () {
      var self = this;
      self.index = 0;
      this.box.on('swipeLeft', function () {
        self.index++;
        if(self.index < 0){
          if(self.setting.loop){
            self.index = self.wNum-1;
            self.go_index(self.index)
          }else{
            self.index = 0
          }
        }else if(self.index > self.wNum-1){
          if(self.setting.loop){
            self.index = 0;
            self.go_index(self.index)
          }else{
            self.index = self.wNum-1
          }
        }else if(self.index > 0 && self.index < self.wNum){
          self.go_index(self.index)
        }
      });
      this.box.on('swipeRight', function () {
        self.index--;
        if(self.index < 0){
          if(self.setting.loop){
            self.index = self.wNum-1;
            self.go_index(self.index)
          }
          else{
            self.index = 0;
          }
        }else if(self.index > self.wNum-1){
          if(self.setting.loop){
            self.index = 0;
            self.go_index(self.index)
          }else{
            self.index = self.wNum-1;
          }
        }else if(self.index > -1 && self.index < self.wNum-1){
          self.go_index(self.index)
        }
      });
      if(self.setting.auto){
        self.autoPlay()
      }
    },
    go_index: function (ind) {
      var self = this;
      clearInterval(this.timer);
      this.box.css('left',-this.winWidth*ind);
      this.box.css(this.getAnimate());

      var dot = this.dotWrap.find('span');
      dot.eq(ind).addClass('active').siblings('.active').removeClass('active');
      if(!!self.setting.auto){
        setTimeout(self.autoPlay(), self.setting.playTime);
      }
    },
    autoPlay: function () {
      var self = this;
      self.timer = setInterval(function () {
        self.box.trigger('swipeLeft')
      },self.setting.playTime);
      return self.timer
    },
    getAnimate: function () {
      return {
        '-webkit-transition':'left'+this.setting.scrollTime+'ms',
        'transition':'left '+this.setting.scrollTime+'ms'
      };
    }
  };
  window['swipeMobile'] = swipeMobile;
})(Zepto);
```