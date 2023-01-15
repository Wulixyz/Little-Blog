<template>
    <Content>
      <div class="row">
        <div class="col-3">
          <UserprofileInfo @follow="follow" @unfollow="unfollow" :user="user"/>
          <UserprofileWrite @SubmitPost="SubmitPost" v-if="is_me"/>
        </div>
        <div class="col-9">
          <UserprofilePost :post="post" @delete_a_post="delete_a_post" :user="user"/>
        </div>
      </div>
    </Content>
</template>

<script>
  import Content from '@/components/ContentBase.vue';
  import UserprofileInfo from '@/components/UserprofileInfo.vue';
  import UserprofilePost from '@/components/UserprofilePost.vue';
  import UserprofileWrite from '@/components/UserprofileWrite.vue';
  import { reactive } from 'vue';
  import { useRoute } from 'vue-router';
  import $ from 'jquery';
  import { useStore } from 'vuex';
  import { computed } from 'vue';

  export default {
    name: 'UserProfile',
    components: {
      Content,
      UserprofileInfo,
      UserprofilePost,
      UserprofileWrite,
    },
    setup() {
      const store = useStore();
      const route = useRoute();
      const userId = parseInt(route.params.userId);
      const user = reactive({});
      const post = reactive({});

      $.ajax({
        url: "https://app165.acapp.acwing.com.cn/myspace/getinfo/",
        type: "GET",
        data: {
          user_id: userId,
        },
        headers: {
          'Authorization': "Bearer " + store.state.user.access,
        },
        success(resp) {
          user.id = resp.id;
          user.username = resp.username;
          user.photo = resp.photo;
          user.followerCount = resp.followerCount;
          user.is_followed = resp.is_followed;
        },
      });

      $.ajax({
        url: "https://app165.acapp.acwing.com.cn/myspace/post/",
        type: "GET",
        data: {
          user_id: userId,
        },
        headers: {
          'Authorization': "Bearer " + store.state.user.access,
        },
        success(resp) {
          post.count = resp.length;
          post.posts = resp;
        },
      });

      const follow = () => {
        if(user.is_followed) return;
        user.is_followed = true;
        user.followerCount ++;
      };

      const unfollow = () => {
        if(!user.is_followed) return;
        user.is_followed = false;
        user.followerCount --;
      };

      const SubmitPost = (content) => {
        post.count ++;
        post.posts.unshift({
          id: post.count,
          userId: 1,
          content: content,
        });
      }

      const delete_a_post = postId => {
        post.posts = post.posts.filter(post => post.id !== postId);
        post.count = post.posts.length;
      }

      const is_me = computed(() => userId === store.state.user.id);

      return {
        user,
        follow,
        unfollow,
        post,
        SubmitPost,
        is_me,
        delete_a_post,
      };
    }
  }
</script>


<style scoped>

</style>