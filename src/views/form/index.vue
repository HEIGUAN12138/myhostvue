<template>
  <div class="app-container">
    <el-header class="header">
      <div class="header-content">
        <div class="logo">
          <h1 class="logo-text">图片社区</h1>
        </div>
        <nav class="nav">
          <el-link type="primary" :underline="false">首页</el-link>
          <el-link type="primary" :underline="false">探索</el-link>
          <el-link type="primary" :underline="false">收藏</el-link>
          <el-link type="primary" :underline="false">关于我们</el-link>
        </nav>
        <div class="header-right">
          <el-input
            v-model="searchText"
            placeholder="搜索图片..."
            prefix-icon="el-icon-search"
            class="search-input"
          />
          <el-button type="primary" class="upload-button">上传图片</el-button>
          <el-button class="notification-button">
            <i class="el-icon-bell"></i>
          </el-button>
          <el-avatar :size="40" :src="avatarUrl" class="avatar" />
        </div>
      </div>
    </el-header>

    <main class="main-content">
      <div class="swiper-container">
        <swiper 
          :options="swiperOption" 
          class="swiper"
          v-swiper:mySwiper="swiperOption">
          <swiper-slide v-for="(slide, index) in slides" :key="index">
            <img :src="slide.image" class="swiper-image" :alt="`轮播图${index + 1}`">
          </swiper-slide>
        </swiper>
      </div>

      <section class="section">
        <h2 class="section-title">热门分类</h2>
        <div class="category-grid">
          <div v-for="category in categories" :key="category.id" class="category-card">
            <img :src="category.image" class="category-image" :alt="category.title">
            <div class="category-info">
              <h3 class="category-title">{{ category.title }}</h3>
              <p class="category-description">{{ category.description }}</p>
            </div>
          </div>
        </div>
      </section>

      <section class="section bg-white">
        <h2 class="section-title">推荐作品</h2>
        <div class="work-grid">
          <div v-for="work in recommendedWorks" :key="work.id" class="work-card">
            <img :src="work.image" class="work-image" :alt="work.title">
            <div class="work-overlay">
              <p class="work-title">{{ work.title }}</p>
              <p class="work-photographer">摄影师：{{ work.photographer }}</p>
            </div>
          </div>
        </div>
      </section>

      <section class="section">
        <h2 class="section-title">摄影师推荐</h2>
        <div class="photographer-grid">
          <div v-for="photographer in photographers" :key="photographer.id" class="photographer-card">
            <el-avatar :size="96" :src="photographer.avatar" class="photographer-avatar" />
            <h3 class="photographer-name">{{ photographer.name }}</h3>
            <p class="photographer-title">{{ photographer.title }}</p>
            <p class="photographer-stats">作品：{{ photographer.works }} 关注：{{ photographer.followers }}</p>
            <el-button type="primary" class="follow-button">关注</el-button>
          </div>
        </div>
      </section>

      <section class="section bg-white">
        <h2 class="section-title">最新活动</h2>
        <div class="activity-grid">
          <div v-for="activity in activities" :key="activity.id" class="activity-card">
            <h3 class="activity-title">{{ activity.title }}</h3>
            <p v-for="(detail, index) in activity.details" :key="index" class="activity-detail">{{ detail }}</p>
            <el-button type="primary" class="activity-button">{{ activity.buttonText }}</el-button>
          </div>
        </div>
      </section>
    </main>

    <footer class="footer">
      <div class="footer-content">
        <div class="footer-section">
          <h3 class="footer-logo">图片社区</h3>
          <p class="footer-description">分享精彩，定格美好</p>
        </div>
        <div v-for="(menu, index) in footerMenus" :key="index" class="footer-section">
          <h4 class="footer-title">{{ menu.title }}</h4>
          <ul class="footer-links">
            <li v-for="(item, itemIndex) in menu.items" :key="itemIndex">
              <a href="#" class="footer-link">{{ item }}</a>
            </li>
          </ul>
        </div>
        <div class="footer-section">
          <h4 class="footer-title">关注我们</h4>
          <div class="social-icons">
            <a v-for="icon in socialIcons" :key="icon" href="#" class="social-icon">
              <i :class="`el-icon-${icon.toLowerCase()}`"></i>
            </a>
          </div>
        </div>
      </div>
      <div class="footer-bottom">
        <p>© 2024 图片社区. 保留所有权利</p>
      </div>
    </footer>
  </div>
</template>

<script>
import { Swiper, SwiperSlide, directive } from 'vue-awesome-swiper'
import 'swiper/swiper-bundle.css'

export default {
  name: 'HomePage',
  components: {
    Swiper,
    SwiperSlide
  },
  directives: {
    swiper: directive
  },
  data() {
    return {
      searchText: '',
      swiperOption: {
        autoplay: {
          delay: 4000,
          disableOnInteraction: false
        },
        pagination: {
          clickable: true
        }
      },
      avatarUrl: 'https://ai-public.mastergo.com/ai/img_res/4aa394720c8dde7b76d3c79b97304c9d.jpg',
      slides: [
        {
          image: 'https://ai-public.mastergo.com/ai/img_res/71015cb534db2052724b7243caaf4465.jpg'
        },
        {
          image: 'https://ai-public.mastergo.com/ai/img_res/381231d47e4fd706ab4b5992e6fc63a0.jpg'
        },
        {
          image: 'https://ai-public.mastergo.com/ai/img_res/a6f103fdb44453751f3b09d9c5c1cd11.jpg'
        }
      ],
      categories: [
        {
          id: 1,
          title: '摄影器材',
          description: '探索专业摄影设备',
          image: 'https://ai-public.mastergo.com/ai/img_res/b0fded567b25be8923764e34f95c9d8f.jpg'
        },
        {
          id: 2,
          title: '自然风光',
          description: '感受大自然的魅力',
          image: 'https://ai-public.mastergo.com/ai/img_res/9d71e96dc3779cee9cb621a643f7ed7b.jpg'
        },
        {
          id: 3,
          title: '人像摄影',
          description: '捕捉精彩瞬间',
          image: 'https://ai-public.mastergo.com/ai/img_res/30e3ed748faf05459e556d203b43bd56.jpg'
        },
        {
          id: 4,
          title: '街头摄影',
          description: '记录城市生活',
          image: 'https://ai-public.mastergo.com/ai/img_res/1c74853de062f721fe76eba8bd4446c9.jpg'
        }
      ],
      recommendedWorks: [
        {
          id: 1,
          title: '晚霞山水',
          photographer: '赵雨晨',
          image: 'https://ai-public.mastergo.com/ai/img_res/f6fbd774a9f0de80c6b1b7318e530f9a.jpg'
        },
        {
          id: 2,
          title: '岁月印记',
          photographer: '林思远',
          image: 'https://ai-public.mastergo.com/ai/img_res/5f9aaa6cea2c912a2e9579f9f565ae3c.jpg'
        },
        {
          id: 3,
          title: '晨露花语',
          photographer: '陈梦琪',
          image: 'https://ai-public.mastergo.com/ai/img_res/0365ee45d62100f5a6644f832e4aab2c.jpg'
        }
      ],
      photographers: [
        {
          id: 1,
          name: '王志远',
          title: '风光摄影师',
          works: 238,
          followers: '12.5k',
          avatar: 'https://ai-public.mastergo.com/ai/img_res/b8205acb933586ac8b0152a647f908a7.jpg'
        },
        {
          id: 2,
          name: '李雨桐',
          title: '人像摄影师',
          works: 186,
          followers: '8.9k',
          avatar: 'https://ai-public.mastergo.com/ai/img_res/77210a602e772357d1636d8c4880f52a.jpg'
        },
        {
          id: 3,
          name: '张明远',
          title: '街拍摄影师',
          works: 342,
          followers: '15.2k',
          avatar: 'https://ai-public.mastergo.com/ai/img_res/c0307f5527f1e475c6f0a674ec3d2633.jpg'
        },
        {
          id: 4,
          name: '刘芳菲',
          title: '微距摄影师',
          works: 156,
          followers: '6.8k',
          avatar: 'https://ai-public.mastergo.com/ai/img_res/8691f9458bffb9cae1a413307402259a.jpg'
        }
      ],
      activities: [
        {
          id: 1,
          title: '2024 年度摄影大赛',
          details: [
            '主题：城市与自然的对话',
            '截止日期：2024 年 6 月 30 日',
            '奖金池：￥100,000'
          ],
          buttonText: '立即报名'
        },
        {
          id: 2,
          title: '春日摄影工作坊',
          details: [
            '地点：杭州西湖',
            '时间：2024 年 4 月 15 日',
            '名额：限 20 人'
          ],
          buttonText: '预约参加'
        }
      ],
      footerMenus: [
        {
          title: '关于我们',
          items: ['公司简介', '加入我们', '联系方式']
        },
        {
          title: '帮助中心',
          items: ['使用指南', '常见问题', '意见反馈']
        }
      ],
      socialIcons: ['wechat', 'envelope', 'camera']
    }
  }
}
</script>

<style scoped>
.header {
  background-color: #ffffff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 0 20px;
}

.header-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: 1200px;
  margin: 0 auto;
  padding: 10px 0;
}

.logo-text {
  font-size: 24px;
  font-weight: bold;
  color: #409EFF;
}

.nav {
  display: flex;
  gap: 20px;
}

.header-right {
  display: flex;
  align-items: center;
  gap: 20px;
}

.search-input {
  width: 300px;
}

.upload-button {
  background-color: #409EFF;
  color: #ffffff;
}

.notification-button {
  background-color: transparent;
  border: none;
}

.avatar {
  cursor: pointer;
}

.main-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.swiper-container {
  margin-top: 20px;
}

.swiper {
  height: 500px;
}

.swiper-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.section {
  margin-top: 40px;
}

.section-title {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 20px;
}

.category-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

.category-card {
  background-color: #ffffff;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.category-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.category-info {
  padding: 20px;
}

.category-title {
  font-size: 18px;
  font-weight: bold;
}

.category-description {
  color: #666666;
  margin-top: 10px;
}

.work-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}

.work-card {
  position: relative;
  border-radius: 8px;
  overflow: hidden;
}

.work-image {
  width: 100%;
  height: 300px;
  object-fit: cover;
}

.work-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  opacity: 0;
  transition: opacity 0.3s;
}

.work-card:hover .work-overlay {
  opacity: 1;
}

.work-title {
  font-size: 20px;
  font-weight: bold;
  color: #ffffff;
}

.work-photographer {
  color: #ffffff;
  margin-top: 10px;
}

.photographer-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

.photographer-card {
  background-color: #ffffff;
  border-radius: 8px;
  padding: 20px;
  text-align: center;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.photographer-avatar {
  margin-bottom: 20px;
}

.photographer-name {
  font-size: 18px;
  font-weight: bold;
}

.photographer-title {
  color: #666666;
  margin-top: 10px;
}

.photographer-stats {
  color: #666666;
  margin-top: 10px;
}

.follow-button {
  margin-top: 20px;
}

.activity-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
}

.activity-card {
  background-color: #ffffff;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.activity-title {
  font-size: 20px;
  font-weight: bold;
}

.activity-detail {
  color: #666666;
  margin-top: 10px;
}

.activity-button {
  margin-top: 20px;
}

.footer {
  background-color: #2d2d2d;
  color: #ffffff;
  padding: 40px 20px;
}

.footer-content {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

.footer-section {
  text-align: left;
}

.footer-logo {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 10px;
}

.footer-description {
  color: #999999;
}

.footer-title {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 10px;
}

.footer-links {
  list-style: none;
  padding: 0;
}

.footer-link {
  color: #999999;
  text-decoration: none;
  display: block;
  margin-bottom: 5px;
}

.footer-link:hover {
  color: #ffffff;
}

.social-icons {
  display: flex;
  gap: 10px;
}

.social-icon {
  color: #999999;
  font-size: 20px;
}

.social-icon:hover {
  color: #ffffff;
}

.footer-bottom {
  text-align: center;
  margin-top: 20px;
  padding-top: 20px;
  border-top: 1px solid #444444;
  color: #999999;
}
</style>