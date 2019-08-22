<template>
<div>
    <!--数据未显示时-->
    <div class="loading" v-if="isLoading">
        <img src="" alt="正在加载数据">
    </div>
    <div else>
        <ul>
            <li v-for='post in posts'>
                <img src="post.author.avatar_url" alt="">
            </li>
        </ul>
    </div>
</div>
</template>

<script>
    export default{
        name: 'PostList',
        data(){
            return {
                isLoading: false,
                posts:[] // 数据列表
            }
        },
        methods:{
            getData(){
                this.$http.get('https://cnodejs.org/api/v1/topics',{
                    page:1,
                    limit:20
                })
                .then(res=>{
                    this.isLoading = false
                    console.log(res)
                    this.posts = res.data.data
                })
                .catch(err=>{

                })
            }
        },
        beforeMount(){
            this.isLoading = true
            this.getData()
        }
    }
</script>