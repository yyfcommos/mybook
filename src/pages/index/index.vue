<template>
  <div class="container">
    <div class="userinfo">
      <img class="userinfo-avatar" v-if="userInfo.avatarUrl" :src="userInfo.avatarUrl" background-size="cover" />
      <div class="userinfo-nickname">
        <card :text="userInfo.nickName"></card>
      </div>
    </div>

    <button class="btn" open-type="getUserInfo" lang="zh_CN" @getuserinfo="doLogin">获取用户信息</button>
  </div>
</template>

<script>
import card from '@/components/card'
import config from '@/config.js'
import qcloud from 'wafer2-client-sdk'
import { showSuccess } from '@/utils/index'

export default {
  data () {
    return {
      userInfo: {
        avatarUrl: 'http://image.shengxinjing.cn/rate/unlogin.png',
        nickName: '...'
      }
    }
  },

  components: {
    card
  },

  methods: {
    // bindViewTap () {
    //   const url = '../logs/main'
    //   wx.navigateTo({ url })
    // },
    getUserInfo () {
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo
              console.log('调用用户信息成功')
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

.userinfo
  display flex
  flex-direction column
  align-items center
  margin-bottom 100rpx
  .userinfo-avatar
    width 220rpx
    height 220rpx
    border-radius 50%










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
