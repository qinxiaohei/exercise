#### 1、0.5px边框

    div {
      border: 1px solid #bbb;
    }
    .hairlines div {
      border-width: 0.5px;
    }
    
    优点：

    简单，不需要过多代码。
    
    缺点：
    
    无法兼容安卓设备、 iOS 8 以下设备。
    
#### 2、使用border-image实现

    .border-image-1px {
      border-bottom: 1px solid #666;
    }
    
    @media only screen and (-webkit-min-device-pixel-ratio: 2) {
      .border-image-1px {
        border-bottom: none;
        border-width: 0 0 1px 0;
        -webkit-border-image: url(../img/linenew.png) 0 0 2 0 stretch;
        border-image: url(../img/linenew.png) 0 0 2 0 stretch;
      }
    }
    
    优点：
    
    可以设置单条,多条边框
    没有性能瓶颈的问题
    
    缺点：
    
    修改颜色麻烦, 需要替换图片
    圆角需要特殊处理，并且边缘会模糊
    
#### 3、使用background-image实现

        .background-image-1px {
          background: url(../img/line.png) repeat-x left bottom;
          -webkit-background-size: 100% 1px;
          background-size: 100% 1px;
        }
        
        优点：

        可以设置单条,多条边框
        没有性能瓶颈的问题
        
        缺点：
        
        修改颜色麻烦, 需要替换图片
        圆角需要特殊处理，并且边缘会模糊
        
#### 4、多背景渐变实现

    .background-gradient-1px {
      background:
        linear-gradient(#000, #000 100%, transparent 100%) left / 1px 100% no-repeat,
        linear-gradient(#000, #000 100%, transparent 100%) right / 1px 100% no-repeat,
        linear-gradient(#000,#000 100%, transparent 100%) top / 100% 1px no-repeat,
        linear-gradient(#000,#000 100%, transparent 100%) bottom / 100% 1px no-repeat
    }
    /* 或者 */
    .background-gradient-1px{
      background:
        -webkit-gradient(linear, left top, right bottom, color-stop(0, transparent), color-stop(0, #000), to(#000)) left / 1px 100% no-repeat,
        -webkit-gradient(linear, left top, right bottom, color-stop(0, transparent), color-stop(0, #000), to(#000)) right / 1px 100% no-repeat,
        -webkit-gradient(linear, left top, right bottom, color-stop(0, transparent), color-stop(0, #000), to(#000)) top / 100% 1px no-repeat,
        -webkit-gradient(linear, left top, right bottom, color-stop(0, transparent), color-stop(0, #000), to(#000)) bottom / 100% 1px no-repeat
    }
    
    优点：
    
    可以实现单条、多条边框
    边框的颜色随意设置
    
    缺点：
    
    代码量不少
    圆角没法实现
    多背景图片有兼容性问题
    
#### 5、使用box-shadow模拟边框

    .box-shadow-1px {
      box-shadow: inset 0px -1px 1px -1px #c8c7cc;
    }
    
    优点：
    
    代码量少
    可以满足所有场景
    
    缺点：
    
    边框有阴影，颜色变浅
    
#### 6、viewport + rem 实现

    在devicePixelRatio = 2 时，输出viewport：
    <meta name="viewport" content="initial-scale=0.5, maximum-scale=0.5, minimum-scale=0.5, user-scalable=no">
    
    在devicePixelRatio = 3 时，输出viewport：
    <meta name="viewport" content="initial-scale=0.3333333333333333, maximum-scale=0.3333>
    
    优点：

    所有场景都能满足
    一套代码，可以兼容基本所有布局
    
    缺点：
    
    老项目修改代价过大，只适用于新项目
    
#### 7、伪类 + transform 实现

    单条border样式设置：
    
    .scale-1px{
      position: relative;
      border:none;
    }
    .scale-1px:after{
      content: '';
      position: absolute;
      bottom: 0;
      background: #000;
      width: 100%;
      height: 1px;
      -webkit-transform: scaleY(0.5);
      transform: scaleY(0.5);
      -webkit-transform-origin: 0 0;
      transform-origin: 0 0;
    }
    
    四条boder样式设置:
    .scale-1px{
      position: relative;
      margin-bottom: 20px;
      border:none;
    }
    .scale-1px:after{
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      border: 1px solid #000;
      -webkit-box-sizing: border-box;
      box-sizing: border-box;
      width: 200%;
      height: 200%;
      -webkit-transform: scale(0.5);
      transform: scale(0.5);
      -webkit-transform-origin: left top;
      transform-origin: left top;
    }
    最好在使用前也判断一下，结合 JS 代码，判断是否 Retina 屏：
    
    if(window.devicePixelRatio && devicePixelRatio >= 2){
      document.querySelector('ul').className = 'scale-1px';
    }
    
    优点：
    
    所有场景都能满足
    支持圆角(伪类和本体类都需要加border-radius)
    缺点：
    
    对于已经使用伪类的元素(例如clearfix)，可能需要多层嵌套