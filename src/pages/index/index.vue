<template>
  <div style="height: 100%">
    <img src="../../../static/images/bg3.jpg" :style="{width: '100%', height: height + 30 + 'px', float: 'left'}">
    <canvas  canvas-id="canvas" :style="{width: width + 'px' , height: height+'px'}"></canvas>
    <scroll-view class="scroll-box" scroll-x="true" @touchstart="touchStart" @touchend="touchEnd">
      <div class="card-box">
        <card :bgPath="bgPath" v-on:url="getUrl"></card>
      </div>
    </scroll-view>
    <button  @click="uploadImg">上传图片</button>
    <button @click="saveImge">保存</button>
  </div>
</template>

<script>
  import card from '../../components/imgCard'
  export default {
    name: 'home',
    data () {
      return {
        ctx: null,
        width: 300,
        height: 400,
        imgWidth: 0,
        imgHeight: 0,
        path: '',
        tempFilePaths: '',
        bgPath: [
          '../../static/images/lamb.png',
          '../../static/images/panda.png',
          '../../static/images/flag.png'
        ],
        selectBg: '',
        startPageX: 0,
        currentView: 0,
        movieDistance: 0
      }
    },
    components: { card },
    // created () {
    //   this.downloadImg()
    // },
    mounted () {
      this.ctx = wx.createCanvasContext('canvas')
      const that = this
      wx.getSystemInfo({
        success (res) {
          that.width = res.windowWidth
        }
      })
      this.drawRadius(this.width / 2 - 100, 30, 200, 200, 3)
      this.drawBg()
      // this.drawImg()
      // this.ctx.draw()
    },
    methods: {
      downloadImg () {
        const that = this
        wx.downloadFile({
          url: that.path,
          success (res) {
            // 只要服务器有响应数据，就会把响应内容写入文件并进入 success 回调，业务需要自行判断是否下载到了想要的内容
            if (res.statusCode === 200) {
              wx.playVoice({
                filePath: res.tempFilePath
              })
            }
          }
        })
      },
      uploadImg () {
        const that = this
        wx.chooseImage({
          count: 1,
          sizeType: ['original', 'compressed'],
          sourceType: ['album', 'camera'],
          success: res => {
            wx.showToast({
              title: '正在上传...',
              icon: 'loading',
              mask: true,
              duration: 1000
            })
            console.log(res)
            // 返回选定照片的本地文件路径列表，tempFilePath可以作为img标签的src属性显示图片
            that.tempFilePaths = res.tempFilePaths[0]
            console.log(res.tempFilePaths)
            // that.drawBg(res.tempFilePaths[0])
            that.drawBg()
            // this.ctx.draw()
            // that.showImg()
          }
        })
      },
      showImg () {
        wx.previewImage({
          urls: this.tempFilePaths,
          success: res => {
            console.log(res)
          },
          fail: err => {
            console.log(err)
          }
        })
      },
      drawImg () {
        const that = this
        // https://i.loli.net/2017/08/21/599a521472424.jpg
        const path = '../../static/images/user.png'
        wx.getImageInfo({
          src: path,
          success: res => {
            console.log(res, that)
            let maxWidth = Math.min(res.width, that.width * 0.65)
            let radio = maxWidth / res.width
            that.width = res.width * radio + 6
            that.height = res.height * radio + 6
            that.imgWidth = res.width * radio
            that.imgHeight = res.height * radio
            // 绘制canvas背景
            // that.ctx.setFillStyle('red')
            // that.ctx.fillRect(0, 0, that.width, that.height)
            that.ctx.drawImage(path, 4, 4, res.width * radio, res.height * radio)
            that.ctx.drawImage(path, res.width * radio - 40, res.height * radio - 40, 40, 40)
            that.drawRadius(4, 4, res.width * radio, res.height * radio, 3)
            that.drawRadius(res.width * radio - 40, res.height * radio - 40, 40, 40, 3)
            that.ctx.draw()
          }
        })
      },
      drawRadius (x, y, w, h, r) {
        this.ctx.beginPath()
        this.ctx.setStrokeStyle('#fff')
        this.ctx.setFillStyle('#fff')
        this.ctx.setLineWidth(r)
        this.ctx.moveTo(x + r, y)
        this.ctx.arcTo(x + w, y, x + w, y + h, r)
        this.ctx.arcTo(x + w, y + h, x, y + h, r)
        this.ctx.arcTo(x, y + h, x, y, r)
        this.ctx.arcTo(x, y, x + w, y, r)
        this.ctx.stroke()
        this.ctx.closePath()
      },
      saveImge () {
        wx.showLoading({
          title: '正在保存图片..'
        })
        const that = this
        wx.canvasToTempFilePath({
          x: that.width / 2 - 100 - 2,
          y: 30 - 2,
          width: 206,
          height: 206,
          canvasId: 'canvas',
          success: res => {
            console.log(res)
            wx.saveImageToPhotosAlbum({
              filePath: res.tempFilePath,
              success (val) {
                console.log(val)
                wx.showToast({
                  title: '保存到相册成功',
                  duration: 1500
                })
              },
              fail (err) {
                console.log(err)
                wx.showToast({
                  title: '保存到相册失败',
                  icon: 'fail'
                })
              },
              complete (res) {
                console.log(res)
              }
            })
          }
        })
      },
      onGotUserInfo (e) {
        this.path = e.target.userInfo.avatarUrl
        this.downloadImg()
        console.log(e, e.target.userInfo.avatarUrl)
      },
      drawBg () {
        this.bgPath.forEach((path, i) => {
          // const path = '../../static/images/lamb.png'
          this.ctx.beginPath()
          this.ctx.setFillStyle('#fff')
          this.ctx.fillRect(30 * (i + 1) + 85 * i, 296, 85, 85)
          this.ctx.fill()
          // this.ctx.drawImage(imgPath, this.width / 2 - 100, this.height / 2 - 100, 200, 200)
          this.drawRadius(30 * (i + 1) + 85 * i, 296, 85, 85, 2)
          this.drawRadius(30 * (i + 1) + 85 * i, 296, 30, 30, 2)
          this.ctx.drawImage(path, 30 * (i + 1) + 85 * i, 296, 30, 30)
        })
        if (this.tempFilePaths) {
          this.ctx.drawImage(this.tempFilePaths, this.width / 2 - 100, 30, 200, 200)
          this.drawRadius(this.width / 2 - 100, 30, 200, 200, 3)
        }
        if (this.selectBg) {
          this.ctx.drawImage(this.selectBg, this.width / 2 - 100, 30, 40, 40)
          this.drawRadius(this.width / 2 - 100, 30, 40, 40, 3)
          if (!this.tempFilePaths) {
            this.drawRadius(this.width / 2 - 100, 30, 200, 200, 3)
          }
        }
        this.ctx.draw()
      },
      getUrl (val) {
        console.log('父组件拿到的选择的图片哦' + val)
        this.selectBg = val
        this.drawBg()
      },
      touchStart (e) {
        this.startPageX = e.mp.changedTouches[0].pageX
        console.log(this.startPageX, e)
      },
      touchEnd (e) {
        const moveX = e.mp.changedTouches[0].pageX - this.startPageX
        console.log('moveX' + moveX, e)
        const maxPage = this.bgPath.length - 1
        if (Math.abs(moveX) >= 50) {
          if (moveX > 0) {
            this.currentView = this.currentView !== 0 ? this.currentView - 1 : 0
          } else {
            this.currentView = this.currentView !== maxPage ? this.currentView + 1 : maxPage
          }
        }
      }
      // 执行步骤 上传图片-》绘制图片=>选择样式（先将所有的样式绘制出来，当点击小图的时候，获取到本地小图的路径，然后保存）-》保存图片

    }
  }
</script>

<style scoped>
  .scroll-box{
    width: 100%;
    overflow: hidden;
    white-space: nowrap;
  }
  .card-box{
    width: 100%;
    display: inline-block;
  }
</style>

