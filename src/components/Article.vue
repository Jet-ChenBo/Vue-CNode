<template>
<div class="Article">
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
        <span class="reply_count">{{post.reply_count}} 回复</span>
        <div class="reply">          
            <div v-for="(reply,index) in post.replies" class="reply_list">
                <router-link :to="{
                    name:'user_info',
                    params: {
                        name: reply.author.loginname,
                    }
                }">
                    <img :src="reply.author.avatar_url" alt="" width="30px" height="30px">
                </router-link>
                <router-link :to="{}">
                    <span>{{reply.author.loginname}}</span>
                </router-link>
                <span>{{index+1}}楼</span>
                <!-- <span v-if="reply.ups.length>0">{{reply.ups.length}}</span>
                <span v-else></span> -->
                <p v-html="reply.content" style="font-size:14px;"></p>
            </div>
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
        },
        watch:{
            '$route'(to,from){
                this.getData()
            }
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

    .Article{
        width: 70%;
        float: left;
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

    .reply_count{
        display: block;
        background-color: #f6f6f6;
        padding: 8px;
        font-size: 14px;
    }

    .reply{
        margin-bottom: 20px;
    }

    .reply .reply_list{
        background-color: #fff;
        margin-bottom: 1px;
        padding: 10px 0 30px 10px;
    }

    .reply_list span{
        display: inline-block;
        vertical-align: top;
        font-size: 12px;
        font-weight: bold;
    }
</style>