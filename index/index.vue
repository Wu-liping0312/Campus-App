<template>
  <view class="page">
    <!-- 头部渲染 -->
    <view class="header-bg">
      <text class="welcome">你好，同学！</text>
      <text class="subtitle">欢迎使用校园智能App</text>
    </view>
    
    <!-- 宫格渲染 -->
    <view class="grid">
      <view class="grid-item" @click="goTo('/pages/course/course')">
        <view class="icon-box" style="background: #e6f0ff; color: #007aff;"><text>📅</text></view>
        <text class="item-text">查课表</text>
      </view>
      <view class="grid-item" @click="goTo('/pages/lostfound/lostfound')">
        <view class="icon-box" style="background: #fff3e0; color: #ff9900;"><text>🔍</text></view>
        <text class="item-text">失物招领</text>
      </view>
      <view class="grid-item" @click="goTo('/pages/notice/notice')">
        <view class="icon-box" style="background: #ffe6e6; color: #ff3b30;"><text>📢</text></view>
        <text class="item-text">校园通知</text>
      </view>
      <view class="grid-item" @click="goTo('/pages/classroom/classroom')">
        <view class="icon-box" style="background: #e6f7eb; color: #4cd964;"><text>🏫</text></view>
        <text class="item-text">空教室</text>
      </view>
      <!-- 新增的 AI 助手入口 -->
      <view class="grid-item" @click="goTo('/pages/ai/ai')">
        <view class="icon-box" style="background: #f3e5f5; color: #9c27b0;"><text>🤖</text></view>
        <text class="item-text">AI 助手</text>
      </view>
    </view>
    
    <button @click="logout" class="logout-btn">退出登录</button>
  </view>
</template>

<script>
export default {
  methods: {
    goTo(url) { uni.navigateTo({ url }) },
    logout() {
      uni.removeStorageSync('isLogin')
      uni.reLaunch({ url: '/pages/login/login' })
    }
  }
}
</script>

<style>
.page { background-color: #f5f6fa; min-height: 100vh; }
.header-bg { background: linear-gradient(135deg, #007aff, #00c6ff); padding: 80rpx 40rpx 60rpx; border-radius: 0 0 40rpx 40rpx; box-shadow: 0 10rpx 20rpx rgba(0, 122, 255, 0.2); }
.welcome { font-size: 48rpx; font-weight: bold; color: #fff; display: block; margin-bottom: 10rpx; }
.subtitle { font-size: 28rpx; color: rgba(255,255,255,0.8); }
.grid { display: flex; flex-wrap: wrap; justify-content: space-between; padding: 40rpx; margin-top: -20rpx; }
.grid-item { width: 47%; background-color: #fff; border-radius: 24rpx; padding: 40rpx 0; display: flex; flex-direction: column; align-items: center; margin-bottom: 30rpx; box-shadow: 0 6rpx 20rpx rgba(0,0,0,0.04); }
.icon-box { width: 100rpx; height: 100rpx; border-radius: 50%; display: flex; justify-content: center; align-items: center; font-size: 50rpx; margin-bottom: 20rpx; }
.item-text { font-size: 30rpx; color: #333; font-weight: 500; }
.logout-btn { margin: 40rpx; background-color: #fff; color: #ff3b30; border-radius: 50rpx; border: 1px solid #ffe5e5; }
</style>