<template>
    <!--数据正在加载时-->
    <div class="loading" v-if="isLoading">
        <img src="" alt="正在加载数据">
    </div>
    <div v-else>
        <div class="topic_header">
            <div class="topic_title">
                <span>{{post|tabFormatter}}</span>
                <span>{{post.title}}</span>
            </div>
            <ul class="clearfix">
                <li>·发布于 {{post.create_at|formatDate}}</li>
                <li>·作者 {{post.author.loginname}}</li>
                <li>·{{post.visit_count}}次浏览</li>
                <li>·来自 {{post|tabFormatter}}</li>
            </ul>
        </div>
        <div v-html="post.content" class="topic_content markdown-body"></div>
        <div class="reply">
            <span>回复</span>
            <div v-for="(reply,index) in post.replies">
                <router-link :to=>
                    <img :src="reply.author.avatar_url" alt="">
                </router-link>
                <router-link :to=>
                    <span>{{reply.author.loginname}}</span>
                </router-link>
                <span>{{index+1}}楼</span>
                <!-- <span v-if="reply.ups.length>0">{{reply.ups.length}}</span>
                <span v-else></span> -->
                <p v-html="reply.content"></p>
            </div>
        </div>
    </div>
</template>

<script>
    export default{
        name: 'Aritcle',
        data(){
            return {
                isLoading: false,
                post:{}
            }
        },
        methods:{
            getData(){
                this.$http.get("https://cnodejs.org/api/v1/topic/" + this.$route.params.id)
                .then(res=>{
                    if(res.data.success == true) this.post = res.data.data
                    this.isLoading = false
                })
                .catch(err=>{
                    console.log(err)
                })
            }
        },
        beforeMount(){
            this.isLoading = true
            this.getData()
        }
    }
</script>

<style>
    @import url('../assets/github-markdown.css');

    .clearfix::after{
        content: '';
        display: block;
        clear: both;
    }

    .topic_header{
        background-color: #fff;
        margin-top: 20px;
        padding: 10px;
        margin-bottom: 2px;
        padding-bottom: 0;
    }

    .topic_content{
        background-color: #fff;
        padding: 10px 20px;
    }

    .topic_title span:nth-child(2){
        font-size: 20px;
        font-weight: bold;
    }

    .topic_title span:nth-child(1){
        background-color: #80bd01;
        border-radius: 3px;
        font-size: 14px;
        color: white;
        padding: 0 5px;
    }

    .topic_header ul{
        list-style: none;
    }

    .topic_header ul li{
        float: left;
        padding: 10px 2px;
        font-size: 12px;
        color: gray;
    }
</style>