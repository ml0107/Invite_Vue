<template>
  <div class="container">
    <!--头部头像昵称-->
    <headers></headers>
    <!--中间内容部分-->
    <div class="content">
      <!--拍照地点及内容-->
      <place :placeinfo="placeinfo"></place>
      <!--选择四个场景-->
      <div class="choose" v-if="flag">
        <div class="administrate" @click="changSite('administrate')">
          <img src="/static/1.png" mode="widthFix" class="sitepic" >
          <p class="sitename">行政楼</p>
        </div>
        <div class="library" @click="changSite('library')">
          <img src="/static/2.png" mode="widthFix" class="sitepic" >
          <p class="sitename">图书馆</p>
        </div>
        <div class="activity" @click="changSite('activity')">
          <img src="/static/5.png" mode="widthFix" class="sitepic" >
          <p class="sitename">活动中心</p>
        </div>
        <div class="ground" @click="changSite('ground')">
          <img src="/static/4.png" mode="widthFix" class="sitepic" >
          <p class="sitename">田径场</p>
        </div>
      </div>
      <!--完成按钮、预览按钮、重做按钮-->
      <div class="btn">
        <button class="make" @click="changeflag">{{flag ? '预览' : '重做'}}</button>
        <button class="finsh" @click="submit">完成</button>
      </div>
    </div>
    <!--星空版权声明-->
    <footers></footers>
  </div>
</template>

<script type="text/ecmascript-6">
  import headers from '@/components/header'
  import footers from '@/components/footer'
  import place from '@/components/place'
  import store from '@/utils/store'
  import { utils, http } from '@/utils/index'
  export default {
    data () {
      return {
        flag: true, // 预览、重做按钮
        userInfo: {},
        placeinfo: { // 默认地点以及文案
          pic: '/static/administrate.png',
          text: '大家好，我是xxx,明天我要在行政楼在拍毕业照啦，快来和我一起拍照留念吧，我的电话是xxxx，诚望百忙之中拨冗赏光，吾辈荣幸之至~'
        }
      }
    },
    mounted () {
      let userInfo = wx.getStorageSync('userInfo')
      if (userInfo) {
        this.userInfo = userInfo
        store.commit('setUser', {
          userInfo: userInfo
        })
      }
    },
    methods: {
      // 改变地点
      changSite (e) {
        let site = e
        switch (site) {
          case 'administrate':
            this.placeinfo.pic = '/static/administrate.png'
            this.placeinfo.text = '大家好，我是xxx,明天我要在行政楼在拍毕业照啦，快来和我一起拍照留念吧，我的电话是xxxx，诚望百忙之中拨冗赏光，吾辈荣幸之至~'
            break
          case 'library':
            this.placeinfo.pic = '/static/library.png'
            this.placeinfo.text = '大家好，我是xxx，光阴荏苒，岁月如梭，逝者如斯夫，明天我要在图书馆门口在拍毕业照啦，快来和我一起拍照留念吧，我的电话是xxxx，诚望百忙之中拨冗赏光，吾辈荣幸之至~'
            break
          case 'activity':
            this.placeinfo.pic = '/static/huozhong.png'
            this.placeinfo.text = '大家好，我是xxx，光阴荏苒，岁月如梭，逝者如斯夫，明天我要在学生活动中心在拍毕业照啦，快来和我一起拍照留念吧，我的电话是xxxx，诚望百忙之中拨冗赏光，吾辈荣幸之至~'
            break
          case 'ground':
            this.placeinfo.pic = '/static/ground.png'
            this.placeinfo.text = '大家好，我是xxx，光阴荏苒，岁月如梭，逝者如斯夫，明天我要在田径场在拍毕业照啦，快来和我一起拍照留念吧，我的电话是xxxx，诚望百忙之中拨冗赏光，吾辈荣幸之至~'
        }
      },
      changeflag () {
        this.flag = !this.flag
      },
      // 完成按钮
      async submit () {
        let content = this.placeinfo.text
        let pic = this.placeinfo.pic
        utils.showLoading('制作中...') // 网络延迟提示信息
        await http.post('GetUrl', {scene: this.userInfo.openId}) // 请求场景值小程序码
        http.post('SetInvite', {openId_id: this.userInfo.openId, content: content, pic: pic})
          .then(result => {
            console.log(result)
            utils.hideLoading() // 隐藏提示信息
            utils.toast('制作完成')
            wx.navigateTo({ url: '../index/main' })
          })
          .catch(error => {
            utils.hideLoading() // 隐藏提示信息
            utils.toast('制作失败')
            console.log(error)
          })
      }
    },
    components: {
      headers,
      footers,
      place
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .content
    .choose
      display flex
      margin 60px auto 0 auto
      justify-content space-between
      width 90%
      .administrate
        width 20%
        padding 5px 1% 7.5px
        background-color #00a0e9
        border-radius 5px
        .sitepic
          width 100%
        .sitename
          text-align center
          font-size 15.5px
          font-weight blod
          color #fff
      .library
        width 20%
        padding 5px 1% 7.5px
        background-color #00a0e9
        border-radius 5px
        .sitepic
          width 100%
        .sitename
          text-align center
          font-size 15.5px
          font-weight blod
          color #fff
      .activity
        width 20%
        padding 5px 1% 7.5px
        background-color #00a0e9
        border-radius 5px
        .sitepic
          width 100%
        .sitename
          text-align center
          font-size 16.5px
          font-weight blod
          color #fff
      .ground
        width 20%
        padding 5px 1% 7.5px
        background-color #00a0e9
        border-radius 5px
        .sitepic
          width 100%
        .sitename
          text-align center
          font-size 15.5px
          font-weight blod
          color #fff
    .btn
      display flex
      margin 60px auto 50px auto
      justify-content space-around
      width 90%
      .make
        width 40%
        background-color #00c0ff
        color #007ba3
        font-weight bold
        border-radius 5px
      .finsh
        width 55%
        background-color #fcff25
        color #ff8400
        font-weight bold
        border-radius 5px
</style>
