<template>
  <div class="minio-bucket-view">
    <h1>MinIO 存储桶和文件列表</h1>
    <div v-if="loading" class="loading-spinner">
      <div class="spinner"></div>
      <p>加载中...</p>
    </div>
    <div v-else>
      <div v-if="error" class="error-message">
        {{ error }}
      </div>
      <div v-else>
        <div v-for="bucket in buckets" :key="bucket.bucketName" class="bucket-card">
          <div class="bucket-header">
            <h2>存储桶: {{ bucket.bucketName }}</h2>
            <p class="creation-date">创建时间: {{ bucket.creationDate }}</p>
          </div>
          <div v-if="bucket.files.length > 0" class="file-table-container">
            <table class="file-table">
              <thead>
                <tr>
                  <th>文件名</th>
                  <th>文件类型</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="file in bucket.files" :key="file.fileName" class="file-row">
                  <td>{{ file.fileName }}</td>
                  <td>{{ file.fileType }}</td>
                </tr>
              </tbody>
            </table>
          </div>
          <p v-else class="no-files">该存储桶中没有文件。</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'MinioBucketView',
  data() {
    return {
      loading: true, // 加载状态
      error: '', // 错误信息
      buckets: [], // 存储桶和文件列表
    };
  },
  created() {
    this.fetchBuckets();
  },
  methods: {
    async fetchBuckets() {
      try {
        // 调用后端接口获取存储桶和文件信息
        const response = await axios.get('http://localhost:19001/down/buckets');
        if (response.data.status === 'success') {
          this.buckets = response.data.buckets;
        } else {
          this.error = response.data.message;
        }
      } catch (err) {
        this.error = '请求失败: ' + err.message;
      } finally {
        this.loading = false;
      }
    },
  },
};
</script>

<style scoped>
.minio-bucket-view {
  padding: 20px;
  font-family: 'Arial', sans-serif;
  max-width: 1200px;
  margin: 0 auto;
}

h1 {
  font-size: 28px;
  font-weight: bold;
  color: #333;
  text-align: center;
  margin-bottom: 30px;
}

.loading-spinner {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-top: 50px;
}

.spinner {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

.error-message {
  color: #e74c3c;
  font-weight: bold;
  text-align: center;
  margin-top: 20px;
}

.bucket-card {
  margin-bottom: 20px;
  padding: 20px;
  border-radius: 10px;
  background-color: #fff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s, box-shadow 0.2s;
}

.bucket-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
}

.bucket-header {
  margin-bottom: 20px;
}

.bucket-header h2 {
  font-size: 22px;
  font-weight: bold;
  color: #2c3e50;
  margin-bottom: 5px;
}

.creation-date {
  font-size: 14px;
  color: #7f8c8d;
}

.file-table-container {
  overflow-x: auto;
}

.file-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 10px;
}

.file-table th,
.file-table td {
  padding: 12px;
  border: 1px solid #ddd;
  text-align: left;
}

.file-table th {
  background-color: #3498db;
  color: #fff;
  font-weight: bold;
}

.file-table tr:nth-child(even) {
  background-color: #f9f9f9;
}

.file-row:hover {
  background-color: #f1f1f1;
}

.no-files {
  font-size: 16px;
  color: #7f8c8d;
  text-align: center;
  margin-top: 10px;
}
</style>