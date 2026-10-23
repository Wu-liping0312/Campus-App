<template>
  <view class="page">
    <view class="top-nav">
      <text class="back-btn" @click="goBack">← 返回</text>
      <text class="title">DeepSeek 校园助手</text>
      <text class="placeholder"></text>
    </view>

    <!-- 聊天记录区 -->
    <scroll-view class="chat-box" scroll-y="true" :scroll-top="scrollTop">
      <view v-for="(msg, index) in messages" :key="index" :class="['msg-item', msg.role]">
        <view class="avatar">{{ msg.role === 'user' ? '🧑‍🎓' : '🤖' }}</view>
        <view class="bubble">{{ msg.content }}</view>
      </view>
      <view v-if="isLoading" class="msg-item assistant">
        <view class="avatar">🤖</view>
        <view class="bubble">DeepSeek 正在思考中...</view>
      </view>
    </scroll-view>

    <!-- 底部输入框 -->
    <view class="input-area">
      <input v-model="inputText" placeholder="问我任何校园问题..." @confirm="sendMessage" />
      <button class="send-btn" @click="sendMessage" :disabled="isLoading">发送</button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      inputText: '',
      messages: [
        { role: 'assistant', content: '你好！我是基于 DeepSeek 大模型的校园助手，有什么可以帮你的吗？' }
      ],
      isLoading: false,
      scrollTop: 0,
      apiKey: 'sk-8c0524ee820646ccac4e64f9a64a1be3' 
    }
  },
  methods: {
    goBack() { uni.navigateBack() },
    
    async sendMessage() {
      if (!this.inputText.trim() || this.isLoading) return;
      
      const userMsg = this.inputText;
      this.messages.push({ role: 'user', content: userMsg });
      this.inputText = '';
      this.isLoading = true;
      this.scrollToBottom();

      try {
        // 调用 DeepSeek 官方 API
        const res = await uni.request({
          url: 'https://api.deepseek.com/chat/completions',
          method: 'POST',
          header: {
            'Content-Type': 'application/json',
            'Authorization': `Bearer ${this.apiKey}`
          },
          data: {
            model: 'deepseek-chat', // DeepSeek 的对话模型
            messages: this.messages
          }
        });

        if (res.statusCode === 200 && res.data.choices) {
          const aiReply = res.data.choices[0].message.content;
          this.messages.push({ role: 'assistant', content: aiReply });
        } else {
          this.messages.push({ role: 'assistant', content: 'AI 接口调用失败，请检查 API Key 或余额。' });
        }
      } catch (e) {
        this.messages.push({ role: 'assistant', content: '网络异常，请稍后再试。' });
      } finally {
        this.isLoading = false;
        this.scrollToBottom();
      }
    },

    scrollToBottom() {
      setTimeout(() => {
        this.scrollTop = 999999;
      }, 100);
    }
  }
}
</script>

<style>
.page { padding: 30rpx; background-color: #f5f6fa; min-height: 100vh; display: flex; flex-direction: column; }
.top-nav { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20rpx; }
.back-btn { font-size: 30rpx; color: #007aff; font-weight: bold; }
.title { font-size: 36rpx; font-weight: bold; color: #333; }
.placeholder { width: 60rpx; }

.chat-box { flex: 1; background: #fff; border-radius: 20rpx; padding: 20rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.04); margin-bottom: 20rpx; height: 800rpx; }
.msg-item { display: flex; margin-bottom: 30rpx; }
.msg-item.user { flex-direction: row-reverse; }
.avatar { width: 70rpx; height: 70rpx; background: #eee; border-radius: 50%; display: flex; justify-content: center; align-items: center; font-size: 40rpx; flex-shrink: 0; }
.bubble { max-width: 70%; padding: 20rpx; border-radius: 16rpx; font-size: 28rpx; line-height: 1.5; word-wrap: break-word; }
.user .bubble { background: #007aff; color: #fff; margin-right: 20rpx; border-radius: 16rpx 4rpx 16rpx 16rpx; }
.assistant .bubble { background: #f0f0f0; color: #333; margin-left: 20rpx; border-radius: 4rpx 16rpx 16rpx 16rpx; }

.input-area { display: flex; align-items: center; background: #fff; border-radius: 50rpx; padding: 10rpx 20rpx; box-shadow: 0 4rpx 16rpx rgba(0,0,0,0.04); }
.input-area input { flex: 1; height: 70rpx; font-size: 28rpx; }
.send-btn { margin-left: 20rpx; background: #007aff; color: #fff; font-size: 28rpx; border-radius: 50rpx; padding: 0 30rpx; line-height: 2.5; }
.send-btn[disabled] { background: #ccc; }
</style>