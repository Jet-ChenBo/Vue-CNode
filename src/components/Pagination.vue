<template>
<div class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button v-if="jduge">...</button>
    <button :class="[{currentPage:btn==currentPage},'pagebtn']" 
    v-for="btn in pagebtns"
    @click="changeBtn(btn)">
        {{btn}}
    </button>
    <button @click="changeBtn">下一页</button>

</div>
</template>

<script>
    import $ from 'jquery'

    export default{
        name: 'Pagination',
        data(){
            return{
                pagebtns:[1,2,3,4,5,'...'],
                currentPage:1,
                jduge:false
            }
        },
        methods:{
            changeBtn(page){
                // 点击首页、上一页、下一页
                if(typeof page != 'number'){
                    switch(page.target.innerText){
                        case '上一页':
                            $('button.currentPage').prev().click()
                            break
                        case '下一页':
                            $('button.currentPage').next().click()
                            break
                        case '首页':
                            this.pagebtns = [1,2,3,4,5,'...']
                            this.changeBtn(1)
                            break
                        default:
                            break
                    }
                    return
                }
                this.currentPage = page
                if(page>4) this.jduge = true
                else this.jduge = false
                if(page == this.pagebtns[4]){
                    this.pagebtns.shift() //移除第一个元素
                    this.pagebtns.splice(4,0,this.pagebtns[3]+1) //加上一页
                }
                else if(page == this.pagebtns[0] && page != 1){
                    this.pagebtns.unshift(this.pagebtns[0]-1) //添加一个
                    this.pagebtns.splice(5,1) //移除第五个
                }
                this.$emit('handleList', this.currentPage) //触发父组件的事件
            }
        }
    }
</script>

<style scoped>
    .pagination{
        margin-top: 5px;
        margin-bottom: 20px;
        background-color: #fff;
        padding: 6px 20px;
        border-radius: 5px;
        border: 1px solid #888;
    }

    button{
        color: #778087;
        padding: 0 10px;
        background-color: #fff;
        border: 1px solid #ddd;
        line-height: 20px;
        cursor: pointer;
        outline: none;
    }

    button:hover{
        background-color: #f5f5f5;
    }
    
    .currentPage{
        color: #80bd01;
    }
</style>