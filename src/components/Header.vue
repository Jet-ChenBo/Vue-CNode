<template>
    <div class='header'>
        <router-link :to="{
            name: 'root'
        }">
            <img src="../assets/cnodejs_light.svg" >
        </router-link>
        <ul>
            <li><a href="#">首页</a></li>
            <li><a href="#">新手入门</a></li>
            <li><a href="#">API</a></li>
            <li><a href="#">关于</a></li>
            <li><a href="#">注册</a></li>
            <li><a href="#">注册</a></li>
        </ul>
    </div>
</template>

<script>
    export default{
        name: 'Header'
    }
</script>

<style scoped>
    .header{
        background-color: #444;
        height: 50px;
    }
    img{
        max-width: 120px;
        margin-left: 50px;
        margin-top: 10px;
    }
    ul{
        list-style: none;
        float: right;
        margin: 12px 80px;
    }
    li{
        display: inline-block;
        margin: 0 10px;
    }

    li a:hover{
        color: white;
    }

    a{
        text-decoration: none;
        color: #ccc;
        font-size: 14px;
    }
</style>