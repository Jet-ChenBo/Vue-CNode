<template>
<div class="PostList">
    <!--数据正在加载时-->
    <div class="loading" v-if="isLoading">
        <img src="" alt="正在加载数据">
    </div>
    <!--主题帖子列表-->
    <div class="posts" v-else>
        <ul>
            <li>
                <div class="toobar">
                    <router-link :to="{
                        name: 'topic_type',
                        params:{
                            topictype: 'all'
                        }
                    }">
                        <a>全部</a>
                    </router-link>
                    <router-link :click="btnClick" :to="{
                        name: 'topic_type',
                        params:{
                            topictype: 'good'
                        }
                    }">
                        <a>精华</a>
                    </router-link>
                    <router-link :to="{
                        name: 'topic_type',
                        params:{
                            topictype: 'share'
                        }
                    }">
                        <a>分享</a>
                    </router-link>
                    <router-link :to="{
                        name: 'topic_type',
                        params:{
                            topictype: 'ask'
                        }
                    }">
                        <a>问答</a>
                    </router-link>
                    <router-link :to="{
                        name: 'topic_type',
                        params:{
                            topictype: 'job'
                        }
                    }">
                        <a>招聘</a>
                    </router-link>
                </div>
            </li>
            <li v-for='post in posts'>
                <img :src="post.author.avatar_url" alt="用户头像">
                <span class="reply_and_count">
                    <!--回复数/浏览量-->
                    <span class="reply_counts">{{post.reply_count}}</span>
                    <span style="color:black; font-size: 12px">/</span>
                    <span class="visit_count">{{post.visit_count}}</span>
                </span>
                <span :class="[{put_good:(post.good == true),put_top:(post.top == true),
                'topiclist-tab':(post.good != true && post.top != true)}]">
                    <!--帖子的分类-->
                    <span>
                        {{post | tabFormatter}}
                    </span>
                </span>
                <router-link class="topic_name" :to="{
                    name:'post_content',
                    params:{
                        id:post.id,
                        name:post.author.loginname
                    }
                }">
                    <!--标题-->
                    {{post.title}}
                </router-link>
                <span class="last_reply">
                    <!--最终回复时间-->
                    {{post.last_reply_at|formatDate}}
                </span>
            </li>
        </ul>
        <!--分页-->
        <pagination @handleList="renderList"></pagination>
    </div>
    
</div>
</template>

<script>
    import pagination from './Pagination'
    import $ from 'jquery'

    export default{
        name: 'PostList',
        data(){
            return {
                isLoading: false,
                posts:[], // 数据列表
                postPage: 1
            }
        },
        components:{
            pagination
        },
        methods:{
            getData(){
                this.$http.get('https://cnodejs.org/api/v1/topics',{
                    params:{
                        page:this.postPage,
                        limit:20,
                        tab:this.$route.params.topictype
                    }
                })
                .then(res=>{
                    this.isLoading = false
                    this.posts = res.data.data
                })
                .catch(err=>{

                })
            },
            renderList(value){
                this.postPage = value
                this.getData()
            },
            btnClick(){
                $(this).addClass('active').siblings().removeClass('active')
            }
        },
        beforeMount(){
            this.isLoading = true
            this.getData()
        },
        watch:{
            $route(to,from){
                this.getData()
            }
        }
    }
</script>

<style scoped>
    .posts{
        margin-top: 14px;
    }

    .PostList img{
        width: 30px;
        height: 30px;
        vertical-align: middle; 
    }

    ul{
        padding: 0;
        list-style: none;
        width: 100%;
    }

    li{
        list-style: none;
        margin-bottom: 2px;
        background-color: #fff;
    }

    .toobar{
        padding: 9px;
        font-size: 15px;
    }

    .toobar a{
        padding: 2px;
        color: #80bd01;
        font-size: 14px;
        margin: 0 5px;
        cursor: pointer;
        text-decoration: none;
    }

    .toobar a:hover{
        color: #005580;
    }

    .router-link-active a{
        background-color: #80bd01;
        border-radius: 4px;
        color: white !important;
    }

    ul li:not(:first-child){
        padding: 9px;
        font-size: 15px;
        /* font-family: "Helvetica Neue", "Luuxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti */
    }

    .put_good, .put_top{
        background-color: #80bd01;
        padding: 2px 4px;
        border-radius: 3px;
        -webkit-border-radius: 3px;
        -moz-border-radius: 3px;
        -o-border-radius: 3px;
        color: #fff;
        font-size: 12px;
        margin-right: 10px;
    }

    .topiclist-tab{
        background-color: #e5e5e5;
        color: #999;
        padding: 2px 4px;
        border-radius: 3px;
        -webkit-border-radius: 3px;
        -moz-border-radius: 3px;
        -o-border-radius: 3px;
        font-size: 12px;
    }
    .last_reply{
        float: right;
        color: #778087;
        font-size: 12px;
        white-space: nowrap;
    }
    router-link{
        text-decoration: none;
        color: black;
        cursor: pointer;
    }
    router-link:hover{
        text-decoration: underline;
    }

    .reply_counts{
        color: #9e78c0;
        margin-right: -4px;
    }

    .visit_count {
        color: #b4b4b4;
        margin-left: -4px;
        font-size: 13px;
    }
    .reply_and_count{
        display: inline-block;
        width: 7%;
        text-align: center;
    }

    a.topic_name{
        text-decoration: none;
        color:#333;
    }

    a.topic_name:hover{
        text-decoration: black underline;
    }
</style>