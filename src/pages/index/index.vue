<template>
  <div class="container">
    <div class="userinfo">
      <img class="userinfo-avatar" v-if="userInfo.avatarUrl" :src="userInfo.avatarUrl" background-size="cover" />
      <div class="userinfo-nickname">
        <card :text="userInfo.nickName"></card>
      </div>
    </div>
    <button class="scanbook" v-if="ifLogin" @click="scanBook">添加图书</button>
    <button v-else open-type="getUserInfo" lang="zh_CN" @click="doLogin">点击登录</button>
    <year-progress></year-progress>
  </div>
</template>

<script>
import card from '@/components/card'
import yearProgress from '@/components/yearProgress'
import config from '@/config.js'
import qcloud from 'wafer2-client-sdk'
import { showSuccess } from '@/utils/index'

export default {
  data () {
    return {
      userInfo: {
        avatarUrl: 'http://image.shengxinjing.cn/rate/unlogin.png',
        nickName: '...'
      },
      ifLogin: false
    }
  },

  components: {
    card,
    yearProgress
  },

  methods: {
    // bindViewTap () {
    //   const url = '../logs/main'
    //   wx.navigateTo({ url })
    // },
    scanBook () {
      // 调用扫码api，允许从相机和相册扫码。
      wx.scanCode({
        success (res) {
          console.log(res)
        }
      })
    },
    getUserInfo () {
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo
              this.ifLogin = true
              console.log('调用用户信息成功', res)
            }
          })
        }
      })
    },
    clickHandle (msg, ev) {
      console.log('clickHandle:', msg, ev)
    },
    doLogin: function (e) {
      qcloud.setLoginUrl(config.loginUrl)
      qcloud.login({
        success: function (res) {
          console.log('登录成功123', res)
          // 将用户登录信息存储在本地缓存的指定的key中
          wx.setStorageSync('userInfo', res)
          showSuccess('登录成功')
          this.ifLogin = true
        },
        fail: function (err) {
          console.log('登录失败1111', err)
        }
      })
    }
  },

  created () {
    // 调用应用实例的方法获取全局数据
    this.getUserInfo()
  }
}
</script>

<style lang='stylus' scoped>
.container
  width 750rpx
  .userinfo
    display flex
    flex-direction column
    align-items center
    margin-bottom 50rpx
    .userinfo-avatar
      width 220rpx
      height 220rpx
      border-radius 50%
  .scanbook
    color white
    background: #ea5a49








// .userinfo {
//   display: flex;
//   flex-direction: column;
//   align-items: center;
// }

// .userinfo-avatar {
//   width: 228rpx;
//   height: 228rpx;
//   margin: 20rpx;
//   border-radius: 50%;
// }

// .userinfo-nickname {
//   color: #aaa;
// }

</style>
