<template>
  <div class="upload-container">
    <!-- 文件上传 -->
    <el-card class="upload-card">
      <h2>文件上传</h2>
      <el-upload
        class="upload-demo"
        drag
        action=""
        :auto-upload="false"
        :on-change="handleFileChange"
        :on-remove="handleFileRemove"
        :file-list="fileList"
        :limit="1"
      >
        <i class="el-icon-upload"></i>
        <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
        <div class="el-upload__tip" slot="tip">只能上传单个文件，且不超过 10MB</div>
      </el-upload>

      <el-button
        type="primary"
        :loading="uploading"
        @click="submitUpload"
      >
        {{ uploading ? '上传中...' : '开始上传' }}
      </el-button>

      <div v-if="uploadResult" class="upload-result">
        <el-alert
          :title="uploadResult"
          :type="uploadSuccess ? 'success' : 'error'"
          show-icon
        />
      </div>
    </el-card>

    <!-- 文件列表 -->
    <el-card class="file-list-card">
      <h2>文件列表</h2>
      <el-table :data="files" style="width: 100%">
        <el-table-column prop="fileName" label="文件名" width="180" />
        <el-table-column label="操作" width="180">
          <template #default="scope">
            <el-button type="primary" size="small" @click="downloadFile(scope.row.fileName)">
              下载
            </el-button>
            <el-button type="success" size="small" @click="previewFile(scope.row.fileName)">
              预览
            </el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>

    <!-- 文件预览 -->
    <div v-if="previewVisible" class="preview-container">
      <div class="preview-content">
        <h3>文件预览</h3>
        <!-- 图片预览 -->
        <img
          v-if="previewType === 'image'"
          :src="previewUrl"
          alt="文件预览"
          class="preview-image"
        />
        <!-- PDF 预览 -->
        <iframe
          v-else-if="previewType === 'pdf'"
          :src="previewUrl"
          class="preview-iframe"
        ></iframe>
        <!-- 视频预览 -->
        <video
          v-else-if="previewType === 'video'"
          :src="previewUrl"
          controls
          class="preview-video"
        ></video>
        <!-- 不支持的文件类型 -->
        <span v-else>无法预览该文件</span>
        <el-button type="primary" @click="closePreview">关闭预览</el-button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'FileManagement',
  data() {
    return {
      fileList: [], // 上传文件列表
      uploading: false, // 是否正在上传
      uploadResult: '', // 上传结果
      uploadSuccess: false, // 是否上传成功
      files: [], // 文件列表
      previewVisible: false, // 预览弹窗是否显示
      previewUrl: '', // 文件预览 URL
      previewType: '', // 文件类型（image、pdf、video）
    };
  },
  mounted() {
    this.fetchFiles(); // 页面加载时获取文件列表
  },
  methods: {
    // 文件选择变化
    handleFileChange(file) {
      this.fileList = [file];
    },
    // 移除文件
    handleFileRemove() {
      this.fileList = [];
    },
    // 提交上传
    async submitUpload() {
      if (this.fileList.length === 0) {
        this.$message.warning('请先选择文件');
        return;
      }

      const file = this.fileList[0].raw;
      if (file.size > 10 * 1024 * 1024) {
        this.$message.warning('文件大小不能超过 10MB');
        return;
      }

      this.uploading = true;
      this.uploadResult = '';

      const formData = new FormData();
      formData.append('file', file);

      try {
        const response = await axios.post('http://localhost:19001/uploads/upload', formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        });
        this.uploadResult = response.data;
        this.uploadSuccess = true;
        this.fetchFiles(); // 上传成功后刷新文件列表
      } catch (error) {
        this.uploadResult = '文件上传失败: ' + (error.response?.data?.message || error.message);
        this.uploadSuccess = false;
      } finally {
        this.uploading = false;
      }
    },
    // 获取文件列表
    async fetchFiles() {
      try {
        const response = await axios.get('http://localhost:19001/uploads/files');
        this.files = response.data.map(fileName => ({ fileName }));
      } catch (error) {
        console.error('获取文件列表失败:', error);
      }
    },
    // 下载文件
    async downloadFile(fileName) {
      try {
        const response = await axios.get('http://localhost:19001/uploads/download', {
          params: { fileName },
          responseType: 'blob',
        });

        const url = window.URL.createObjectURL(new Blob([response.data]));
        const link = document.createElement('a');
        link.href = url;
        link.setAttribute('download', fileName);
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
      } catch (error) {
        console.error('文件下载失败:', error);
      }
    },
    // 预览文件
    async previewFile(fileName) {
      try {
        console.log('正在获取文件预览 URL，文件名:', fileName);
        const response = await axios.get('http://localhost:19001/uploads/presigned-url', {
          params: { fileName },
        });
        console.log('获取到的 Presigned URL:', response.data);
        this.previewUrl = response.data;

        // 根据文件扩展名设置文件类型
        const extension = fileName.split('.').pop().toLowerCase();
        if (['jpg', 'jpeg', 'png', 'gif'].includes(extension)) {
          this.previewType = 'image'; // 图片
        } else if (extension === 'pdf') {
          this.previewType = 'pdf'; // PDF
        } else if (extension === 'mp4') {
          this.previewType = 'video'; // 视频
        } else {
          this.previewType = ''; // 不支持的文件类型
        }

        this.previewVisible = true; // 显示预览区域
      } catch (error) {
        console.error('获取文件预览 URL 失败:', error);
        this.previewUrl = '';
        this.previewType = '';
        this.previewVisible = true; // 显示预览区域，提示错误
      }
    },
    // 关闭预览
    closePreview() {
      this.previewVisible = false; // 隐藏预览区域
      this.previewUrl = ''; // 清空预览 URL
      this.previewType = ''; // 清空文件类型
    },
  },
};
</script>

<style scoped>
.file-management-container {
  padding: 20px;
}

.upload-card,
.file-list-card {
  margin-bottom: 20px;
}

h2 {
  margin-bottom: 20px;
  color: #303133;
}

.upload-demo {
  margin-bottom: 20px;
}

.upload-result {
  margin-top: 20px;
}

/* 预览区域样式 */
.preview-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); /* 半透明背景 */
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000; /* 确保在最上层 */
}

.preview-content {
  background-color: white;
  padding: 20px;
  border-radius: 8px;
  text-align: center;
  max-width: 90%;
  max-height: 90vh;
  overflow: auto;
}

.preview-image {
  max-width: 100%;
  max-height: 80vh; /* 限制图片最大高度 */
  border-radius: 8px;
  margin-bottom: 20px;
}

.preview-iframe {
  width: 80vw;
  height: 80vh;
  border: none;
  border-radius: 8px;
  margin-bottom: 20px;
}

.preview-video {
  max-width: 100%;
  max-height: 80vh; /* 限制视频最大高度 */
  border-radius: 8px;
  margin-bottom: 20px;
}
</style>