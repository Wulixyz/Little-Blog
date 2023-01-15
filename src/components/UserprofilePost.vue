<template>
    <div class="card">
        <div class="card-body">
            <div v-for="p in post.posts" :key="p.id">
                <div class="card single-post">
                    <div class="card-body">
                        {{ p.content }}
                        <button v-if="is_me" type="button" class="btn btn-danger btn-sm" @click="delete_a_post(p.id)">刪除</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import { useStore } from 'vuex';
    import { computed } from 'vue';
    import $ from 'jquery';

    export default {
        name: "UserprofilePost",
        props: {
            post: {
                type: Object,
                required: true,
            },
            user: {
                type: Object,
                required: true,
            },
        },
        setup(props,context) {
            const store = useStore();
            let is_me = computed(() => store.state.user.id === props.user.id);

            const delete_a_post = post_id => {
                $.ajax({
                    url: "https://app165.acapp.acwing.com.cn/myspace/post/",
                    type: "DELETE",
                    data: {
                        post_id: post_id,
                    },
                    headers: {
                        'Authorization': "Bearer " + store.state.user.access,
                    },
                    success(resp) {
                        if(resp.result === "success") {
                            context.emit("delete_a_post",post_id);
                        }
                    },
                });
            }

            return {
                is_me,
                delete_a_post,
            }
        }
    }
</script>

<style scoped>
    .single-post {
        margin: 10px;
    }

    button {
        float: right;
    }
</style>