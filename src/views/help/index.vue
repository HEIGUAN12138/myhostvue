<template>
    <div class="image-preview-container">
      <h1>图片预览</h1>
      <div v-if="loading" class="loading">加载中...</div>
      <div v-else>
        <img :src="imageUrl" alt="预览图片" class="preview-image" />
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'ImagePreview',
    data() {
      return {
        imageUrl: '', // 图片的 URL
        loading: true, // 加载状态
      };
    },
    mounted() {
      this.loadImage();
    },
    methods: {
      loadImage() {
        // 设置图片的 URL
        this.imageUrl = 'http://150.158.1.117:9000/picturebucket/-08.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=FDn2lN50Bzr2v2QSDQMy%2F20250119%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20250119T105903Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=8ac876763847bcad510fa1c47699c3d097eba01fde16832486bb342fbadf0a0d';
  
        // 模拟图片加载
        const img = new Image();
        img.src = this.imageUrl;
        img.onload = () => {
          this.loading = false; // 图片加载完成
        };
        img.onerror = () => {
          this.loading = false; // 图片加载失败
          console.error('图片加载失败');
        };
      },
    },
  };
  </script>
  
  <style scoped>
  .image-preview-container {
    text-align: center;
    padding: 20px;
  }
  
  h1 {
    font-size: 24px;
    font-weight: bold;
    margin-bottom: 20px;
  }
  
  .loading {
    font-size: 18px;
    color: #666;
  }
  
  .preview-image {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  </style>