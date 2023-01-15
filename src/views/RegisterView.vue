<template>
  <Content>
    <div class="row justify-content-md-center">
      <div class="col-3">
        <form @submit.prevent="register">
          <div class="mb-3">
            <label for="username" class="form-label">用戶名</label>
            <input v-model="username" type="text" class="form-control" id="username" aria-describedby="emailHelp">
          </div>
          <div class="mb-3">
            <label for="password" class="form-label">密碼</label>
            <input v-model="password" type="password" class="form-control" id="password">
          </div>
          <div class="mb-3">
            <label for="password_confirm" class="form-label">重複密碼</label>
            <input v-model="password_confirm" type="password" class="form-control" id="password_confirm">
          </div>
          <div class="error-message">{{ errorMessage }}</div>
          <button type="submit" class="btn btn-primary">注冊</button>
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
import $ from 'jquery';

export default {
  name: 'RegisterView',
  components: {
    Content,
  },
  setup() {
    const store = useStore();
    let username = ref('');
    let password = ref('');
    let password_confirm = ref('');
    let errorMessage = ref('');

    const register = () => {
      errorMessage.value = '';

      $.ajax({
        url: "https://app165.acapp.acwing.com.cn/myspace/user/",
        type: "POST",
        data: {
          username: username.value,
          password: password.value,
          password_confirm: password_confirm.value,
        },
        success(resp) {
          if(resp.result === "success") {
            store.dispatch('login',{
              username: username.value,
              password: password.value,
              success() {
                router.push({name: 'userlist'});
              },
              error() {
                errorMessage.value = "系統異常，請稍後重試";
              }
            });
          } else {
            errorMessage.value = resp.result;
          }
        }
      });
    }

    return {
      username,
      password,
      password_confirm,
      errorMessage,
      register,
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