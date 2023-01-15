<template>
    <Content>
      <div class="row justify-content-md-center">
        <div class="col-3">
          <form @submit.prevent="login">
            <div class="mb-3">
              <label for="username" class="form-label">用戶名</label>
              <input v-model="username" type="text" class="form-control" id="username" aria-describedby="emailHelp">
            </div>
            <div class="mb-3">
              <label for="password" class="form-label">密碼</label>
              <input v-model="password" type="password" class="form-control" id="password">
            </div>
            <div class="error-message">{{ errorMessage }}</div>
            <button type="submit" class="btn btn-primary">登錄</button>
          </form>
        </div>
      </div>
      
    </Content>
  </template>
  
  <script>
  import Content from '@/components/ContentBase.vue';
  import { ref } from 'vue';
  import { useStore } from 'vuex';
  import router from '@/router';
  
  export default {
    name: 'LoginView',
    components: {
      Content,
    },
    setup() {
      const store = useStore();
      let username = ref('');
      let password = ref('');
      let errorMessage = ref('');

      const login = () => {
        errorMessage.value = "";
        store.dispatch('login',{
          username: username.value,
          password: password.value,
          success() {
            router.push({name: 'userlist'});
          },
          error() {
            errorMessage.value = "用戶名或密碼錯誤";
          }
        });
      }

      return {
        username,
        password,
        errorMessage,
        login,
      }
    }
  }
  </script>
  
  
  <style scoped>

  button {
    width: 100%;
  }

  .error-message {
    color: red;
    font-size: 12px;
    margin-bottom: 10px;
  }
  
  </style>