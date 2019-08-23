<template>
<div class="UserInfo">
    <!--数据正在加载时-->
    <div class="loading" v-if="isLoading">
        <img src="" alt="正在加载数据">
    </div>
    <div class="UserInfomation" v-else>
        <router-link class="toMain" :to="{
            name: 'root',
        }">
            <span class="type">主页/</span>
        </router-link>
        <section>
            <img :src="post.avatar_url" alt="" width="40px" height="40px">
            <span>{{post.loginname}}</span>
            <p>{{post.score}}积分</p>
            <p>注册时间 {{post.create_at|formatDate}}</p>
        </section>
        <span class="type">最近创建的话题</span>
        <div class="create_topic">
            <ul>
                <li v-for="item in post.recent_topics">
                    <img :src="post.avatar_url" alt="" width="30px" height="30px">
                    <router-link :to="{
                        name: 'post_content',
                        params:{
                            id: item.id
                        }
                    }">
                        <span>{{item.title}}</span>
                    </router-link>
                </li>
            </ul>
        </div>
        <span class="type">最近参与的话题</span>
        <div class="reply_topic">
            <ul>
                <li v-for="item in post.recent_replies">
                    <img :src="item.author.avatar_url" alt="" width="30px" height="30px">
                    <router-link :to="{
                        name: 'post_content',
                        params:{
                            id: item.id
                        }
                    }">
                        {{item.title}}
                    </router-link>
                </li>
            </ul>
        </div>
    </div>
</div>
</template>

<script>
    export default{
        name: 'UserInfo',
        data(){
            return {
                isLoading: false,
                post:{} //请求到的数据
            }
        },
        methods:{
            getData(){
                this.$http.get("https://cnodejs.org/api/v1/user/" + this.$route.params.name)
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

<style scoped>
    .UserInfo{
        margin: 20px 0;
    }

    a.toMain{
        color: #80bd01;
        text-decoration: none;
    }

    .type{
        display: block;
        background-color: #f6f6f6;
        padding: 10px;
        font-size: 14px;
    }
    
    section{
        background-color: #fff;
    }

    section img{
        padding: 10px 0 0 10px;
    }

    section span{
        vertical-align: top;
        display: inline-block;
        font-size: 14px;
        color: #778087;
        margin-top: 10px;
    }

    section :nth-child(3){
        font-size: 13px;
        padding-left: 10px;
    }

    section :nth-child(4){
        font-size: 14px;
        color: #778087;
        padding: 10px 10px;
    }


    .create_topic ul, .reply_topic ul{     
        list-style: none;
    }

    .create_topic img, .reply_topic img{
        padding: 10px 0 0 10px;
    }

    .create_topic ul li, .reply_topic ul li{
        margin-bottom: 1px;
        font-size: 14px;
        background-color: #fff;
    }

    .create_topic ul li a, .reply_topic ul li a{
        display: inline-block;
        vertical-align: top;
        margin-top: 15px;
        text-decoration: none;
    }

    .create_topic ul li a:hover, .reply_topic ul li a:hover{
        text-decoration: underline #005580;
        color: #005580;
    }

</style>