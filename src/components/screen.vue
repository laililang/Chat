<template>
    <div id="chat" class="message" v-show="name">
        <div class="message__name">{{ name }}</div>
        <div class="message__post" v-for="post in posts">
            <p>{{ post.author }}: {{ post.content }}</p>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        name: {
            type: String
        }
    },
    data () {
        return {
            posts: []
        }
    },
    methods: {
        publish: function() {
            this.$http.get('https://livechat-b3aa5.firebaseio.com/posts.json').then(function(data){
                return data.json()
            }).then(function(data){
                var postsArray = [];
                for (let key in data){
                    data[key].id = key;
                    postsArray.push(data[key]);
                }
                this.posts = postsArray;
            });
            var chat = document.querySelector('#chat');
            chat.scrollTop = chat.scrollHeight;
        },
        check: function() {
            setInterval(this.publish, 500);
        }
    },
    created() {
        this.check();
    }
}
</script>

<style lang="scss">



</style>
