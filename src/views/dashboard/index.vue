<template>
  <div class="dashboard-container">
    <div class="dashboard-text">name: {{ name }}</div>

    <!-- 新增的表单部分 -->
    <div class="personal-information-form">
      <h2>个人信息提交</h2>
      <form @submit.prevent="submitForm">
        <div class="form-group">
          <label for="name">
            <i class="fas fa-user"></i>
            姓名：
          </label>
          <input type="text" id="name" v-model="formData.name" required />
        </div>

        <div class="form-group">
          <label for="age">
            <i class="fas fa-birthday-cake"></i>
            年龄：
          </label>
          <input type="number" id="age" v-model="formData.age" required />
        </div>

        <div class="form-group">
          <label for="relationship_student">
            <i class="fas fa-user-graduate"></i>
            与学生关系：
          </label>
          <input type="text" id="relationship_student" v-model="formData.relationship_student" required />
        </div>

        <div class="form-group">
          <label for="relationship_friend">
            <i class="fas fa-users"></i>
            与朋友关系：
          </label>
          <input type="text" id="relationship_friend" v-model="formData.relationship_friend" required />
        </div>

        <div class="form-group">
          <label for="phone_number">
            <i class="fas fa-phone"></i>
            电话号码：
          </label>
          <input type="number" id="phone_number" v-model="formData.phone_number" required />
        </div>

        <div class="form-group">
          <label for="address">
            <i class="fas fa-map-marker-alt"></i>
            地址：
          </label>
          <input type="text" id="address" v-model="formData.address" required />
        </div>

        <div class="form-group">
          <label for="hobbys">
            <i class="fas fa-heart"></i>
            爱好：
          </label>
          <input type="text" id="hobbys" v-model="formData.hobbys" required />
        </div>

        <div class="form-group">
          <label for="professional">
            <i class="fas fa-briefcase"></i>
            职业：
          </label>
          <input type="text" id="professional" v-model="formData.professional" required />
        </div>

        <div class="form-group">
          <label for="id">
            <i class="fas fa-id-card"></i>
            ID：
          </label>
          <input type="number" id="id" v-model="formData.id" required />
        </div>

        <button type="submit">提交</button>
      </form>

      <div v-if="message" class="message">
        {{ message }}
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
import axios from 'axios';

export default {
  name: 'Dashboard',
  data() {
    return {
      formData: {
        name: '',
        age: null,
        relationship_student: '',
        relationship_friend: '',
        phone_number: null,
        address: '',
        hobbys: '',
        professional: '',
        id: null,
      },
      message: '', // 用于显示提交结果
    };
  },
  computed: {
    ...mapGetters([
      'name'
    ])
  },
  methods: {
    async submitForm() {
      try {
        // 发送 POST 请求到后端
        const response = await axios.post('http://localhost:19001/api/personal-information/submit', this.formData);
        this.message = response.data; // 显示后端返回的消息
        this.resetForm(); // 清空表单
      } catch (error) {
        this.message = '提交失败，请重试！';
        console.error('提交失败：', error);
      }
    },
    resetForm() {
      // 清空表单数据
      this.formData = {
        name: '',
        age: null,
        relationship_student: '',
        relationship_friend: '',
        phone_number: null,
        address: '',
        hobbys: '',
        professional: '',
        id: null,
      };
    },
  },
};
</script>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700&display=swap');
@import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css');

.dashboard-container {
  margin: 30px;
  font-family: 'Roboto', sans-serif;
}

.dashboard-text {
  font-size: 30px;
  line-height: 46px;
  color: #2c3e50;
  margin-bottom: 20px;
}

.personal-information-form {
  max-width: 500px;
  margin: 0 auto;
  padding: 40px;
  background: #ffffff;
  border-radius: 15px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;

  &:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.15);
  }
}

h2 {
  font-size: 28px;
  color: #42b983;
  margin-bottom: 30px;
  text-align: center;
  font-weight: 700;
}

.form-group {
  margin-bottom: 25px;
}

label {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
  font-weight: 500;
  color: #555;

  i {
    margin-right: 10px;
    color: #42b983;
  }
}

input {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 14px;
  box-sizing: border-box;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;

  &:focus {
    border-color: #42b983;
    outline: none;
    box-shadow: 0 0 8px rgba(66, 185, 131, 0.3);
  }
}

button {
  width: 100%;
  padding: 14px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.3s ease;

  &:hover {
    background-color: #369f6e;
    transform: translateY(-2px);
  }

  &:active {
    transform: translateY(0);
  }
}

.message {
  margin-top: 25px;
  padding: 15px;
  background-color: #e8f5e9;
  color: #2e7d32;
  border-radius: 8px;
  text-align: center;
  font-weight: 500;
  animation: fadeIn 0.5s ease;

  @keyframes fadeIn {
    from {
      opacity: 0;
      transform: translateY(-10px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }
}
</style>