<template>
  <div class="local-video-player-container">
    <el-card class="video-player-card">
      <h2>本地视频播放器</h2>

      <!-- 文件选择 -->
      <div class="file-input">
        <el-button type="primary" @click="openFileDialog">
          选择视频文件
        </el-button>
        <input
          ref="fileInput"
          type="file"
          accept="video/*"
          style="display: none"
          @change="handleFileChange"
        />
      </div>

      <!-- 视频播放器 -->
      <div class="video-player" v-if="videoUrl">
        <video
          ref="videoPlayer"
          controls
          :src="videoUrl"
          width="100%"
          height="auto"
        >
          您的浏览器不支持视频播放。
        </video>
      </div>

      <!-- 错误提示 -->
      <div v-if="errorMessage" class="error-message">
        <el-alert
          :title="errorMessage"
          type="error"
          show-icon
        />
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
  name: 'LocalVideoPlayer',
  data() {
    return {
      videoUrl: '', // 视频文件的 URL
      errorMessage: '', // 错误信息
    };
  },
  methods: {
    // 打开文件选择对话框
    openFileDialog() {
      this.$refs.fileInput.click();
    },

    // 处理文件选择
    handleFileChange(event) {
      const file = event.target.files[0];
      if (!file) {
        this.errorMessage = '未选择文件';
        return;
      }

      // 检查文件类型
      if (!file.type.startsWith('video/')) {
        this.errorMessage = '请选择有效的视频文件';
        return;
      }

      // 生成视频文件的 URL
      this.videoUrl = URL.createObjectURL(file);
      this.errorMessage = '';
    },
  },
};
</script>

<style scoped>
.local-video-player-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f5f7fa;
}

.video-player-card {
  width: 800px;
  padding: 20px;
  text-align: center;
}

h2 {
  margin-bottom: 20px;
  color: #303133;
}

.file-input {
  margin-bottom: 20px;
}

.video-player {
  margin-top: 20px;
}

.error-message {
  margin-top: 20px;
}
</style>