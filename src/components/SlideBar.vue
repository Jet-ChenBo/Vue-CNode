<template>
<div class="slideBar">
    <div class="topbar">作者</div>
    <div class="authorsummary">
        <router-link :to="{
            name:'user_info',
            params: {
                name: post.loginname,
            }
        }">
            <img :src="post.avatar_url" alt="" width="50px" height="50px">
        </router-link>
        <router-link :to="{
            name:'user_info',
            params: {
                name: post.loginname,
            }
        }">
            {{post.loginname}}
        </router-link>
        <p>积分: {{post.score}}</p>
    </div>
    <div class="topbar two">作者其他话题</div>
    <div class="other_topics">
        <ul>
            <Li v-for="list in topiclimited">
                <router-link :to="{
                    name: 'post_content',
                    params:{
                        id: list.id,
                        name: list.author.loginname
                    }
                }">
                    {{list.title}}  
                </router-link>               
            </Li>
        </ul>
    </div>
    <div class="topbar three">作者最近回复</div>
    <div class="recent_replies">
        <ul>
            <Li v-for="list in replylimited">
                <router-link :to="{
                    name: 'post_content',
                    params:{
                        id: list.id,
                        name: list.author.loginname
                    }
                }">
                    {{list.title}}  
                </router-link>  
            </Li>
        </ul>
    </div>
</div>
</template>

<script>
    export default{
        name: 'SledeBar',
        data(){
            return {
                post:{},
            }
        },
        methods:{
            getData(){
                this.$http.get("https://cnodejs.org/api/v1/user/" + this.$route.params.name)
                .then(res=>{
                    if(res.data.success == true){
                        this.post = res.data.data
                        console.log(this.post.recent_replies)
                        for(let i=0; i<this.post.recent_topics.length; i++){
                            if(this.post.recent_topics[i].id === this.$route.params.id){
                                this.post.recent_topics.splice(i,1) //删除当前文章
                            }
                        }
                    }
                })
                .catch(err=>{
                    console.log(err)
                })
            }
        },
        computed:{
            topiclimited(){
                if(this.post.recent_topics){
                    return this.post.recent_topics.slice(0,5)
                }               
            },
            replylimited(){
                if(this.post.recent_replies){
                    return this.post.recent_replies.slice(0,5)
                }               
            }
        },
        beforeMount(){
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
     .slideBar{
        margin-top: 20px;
        margin-left: 1%;
        width: 29%;
        float: right;
    }

    .topbar{
        color: #51585c;
        border-radius: 3px 3px 0 0;
        padding: 10px;
        background-color: #f6f6f6;
        font-size: 13px;
    }

    .two,.three{
        margin-top: 30px;
    }

    .authorsummary{
        background-color: #fff;
        padding: 10px;
    }

    .authorsummary a{
        vertical-align: top;
        line-height: 50px;
        text-decoration: none;
        color: #778087;
    }

    .authorsummary p{
        font-size: 14px;
    }

    .other_topics, .recent_replies{
        background: #fff;
    }

    .other_topics ul, .recent_replies ul{
        list-style: none;
    }

    .other_topics li, .recent_replies li{
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        padding: 5px;
        
    }

    .other_topics li a, .recent_replies li a{
        color: #778087;
        text-decoration: none;
        font-size: 14px;
    }

</style>