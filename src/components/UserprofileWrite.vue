<template>
    <div class="card edit-field">
        <div class="card-body">
            <div class="mb-3">
                <label for="edit-post" class="form-label">編輯帖子</label>
                <textarea v-model="content" class="form-control" id="edit-post" rows="3"></textarea>
            </div>
            <button @click="SubmitPost" type="button" class="btn btn-primary btn-sm">發帖</button>
        </div>
    </div>
</template>

<script>
    import { ref } from 'vue';
    import { useStore } from 'vuex';
    import $ from 'jquery';

    export default {
        name: "UserProfileWrite",
        setup(props,context) {
            const store = useStore();
            let content = ref('');

            const SubmitPost = () => {
                $.ajax({
                    url: "https://app165.acapp.acwing.com.cn/myspace/post/",
                    type: "POST",
                    data: {
                        content: content.value,
                    },
                    headers: {
                        'Authorization': "Bearer " + store.state.user.access,
                    },
                    success(resp) {
                        if(resp.result === "success") {
                            context.emit("SubmitPost",content.value);
                            content.value = "";
                        }
                    }
                });
            }

            return {
                content,
                SubmitPost,
            }
        }
    }
</script>

<style scoped>
    .edit-field {
        margin-top: 20px;
    }
</style>