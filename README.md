# add-flag

> A Mpvue project

## Build Setup

``` bash
# 初始化项目
vue init mpvue/mpvue-quickstart myproject
cd myproject

# 安装依赖
yarn

# 开发时构建
npm dev

# 打包构建
npm build

# 指定平台的开发时构建(微信、百度、头条、支付宝)
npm dev:wx
npm dev:swan
npm dev:tt
npm dev:my

# 指定平台的打包构建
npm build:wx
npm build:swan
npm build:tt
npm build:my

# 生成 bundle 分析报告
npm run build --report

#项目实现目的
为上传图片添加小图标，如国旗，圣诞帽，爱心，小动物等等
#项目实现步骤
1.项目页面，共两个页面，使用介绍页面，使用页面
2.使用页面划分
    图片上传
        使用微信小程序原生API，wx.chooseImage
        详情参见：https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.chooseImage.html
    选择小图标
        将图片都展示出来，并使用微信小程序原生APIview-scroll实现左右滑动
        通过click事件获取图片信息
    选择边框颜色，选择边框样式，圆形，矩形
        将可选择的色块展示出来，默认白色，通过click事件获取颜色信息，边框样式同
    使用canvas为图片添加小图标及其边框
        1.使用微信小程序原生API获取画布wx.createCanvasContext('canvas')
        2.绘制边框，通过一下代码 
                         this.ctx.moveTo(x + r, y)
                         this.ctx.arcTo(x + w, y, x + w, y + h, r)
                         this.ctx.arcTo(x + w, y + h, x, y + h, r)
                         this.ctx.arcTo(x, y + h, x, y, r)
                         this.ctx.arcTo(x, y, x + w, y, r)
                         this.ctx.stroke()
        绘制带圆角的边框
        3.绘制上传图片
            1.使用wx.getImageInfo获取到图片信息，并计算图片宽高比例
            2.宽固定，更具计算的出的图片宽高比列来计算高的大小，避免图片变形
        4.绘制小图标及其边框，原理如上
    保存绘制完成的图片
        使用微信小程序原生API wx.canvasToTempFilePath， wx.saveImageToPhotosAlbum
        将其保存至相册
        API详情参见：https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.saveImageToPhotosAlbum.html
    保存
   使用帮助页面
    自定义


```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
