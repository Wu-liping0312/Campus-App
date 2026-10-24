<template>
  <view class="login-bg">
    <view class="login-card">
      <view class="title">校园智能App</view>
      <view class="subtitle">欢迎回来，请登录</view>
      
      <input v-model="account" placeholder="请输入账号" class="input" />
      <input v-model="password" placeholder="请输入密码" type="password" class="input" />
      
      <button @click="handleLogin" class="btn">登 录</button>
      <button @click="goRegister" class="btn btn-register">注册新账号</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return { account: '', password: '' }
  },
  methods: {
    handleLogin() {
      const saved = uni.getStorageSync('userInfo')
      if (saved && saved.account === this.account && saved.password === this.password) {
        uni.setStorageSync('isLogin', true)
        uni.reLaunch({ url: '/pages/index/index' })
      } else {
        uni.showToast({ title: '账号或密码错误', icon: 'none' })
      }
    },
    goRegister() {
      if (!this.account || !this.password) {
        uni.showToast({ title: '请填写账号密码', icon: 'none' })
        return
      }
      uni.setStorageSync('userInfo', { account: this.account, password: this.password })
      uni.showToast({ title: '注册成功，请点击登录', icon: 'none' })
    }
  }
}
</script>

<style>
.login-bg { min-height: 100vh; background: linear-gradient(135deg, #74ebd5 0%, #9face6 100%); display: flex; align-items: center; justify-content: center; padding: 0 40rpx; }
.login-card { width: 100%; background: #fff; border-radius: 30rpx; padding: 60rpx 40rpx; box-shadow: 0 10rpx 30rpx rgba(0,0,0,0.1); }
.title { font-size: 48rpx; font-weight: bold; color: #333; text-align: center; margin-bottom: 10rpx; }
.subtitle { font-size: 28rpx; color: #999; text-align: center; margin-bottom: 50rpx; }
.input { border: 1px solid #eee; background: #f9f9f9; padding: 24rpx; margin-bottom: 30rpx; border-radius: 12rpx; font-size: 30rpx; }
.btn { margin-bottom: 20rpx; background: #007aff; color: #fff; border-radius: 50rpx; font-size: 32rpx; padding: 10rpx 0; }
.btn-register { background: transparent; color: #007aff; border: 1px solid #007aff; }
</style>