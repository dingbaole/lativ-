<template>
 <div>
     <van-search v-model="value" placeholder="请输入搜索关键词" @keydown.enter="en"/>
     <item :newlist="newlist" />
     <ul>
         <li v-for="(item,index) in ls " :key="index">
             {{ item }}
         </li>
     </ul>
 </div>
</template>

<script>
import item from "../components/item"
export default {
 name: '',
 mounted() {
     this.getlist()
 },
 data() {
 return {
     value:"",
     list:[],
     ls:[]
 }
 },
 components: {item},
 methods: {
     getlist(){
         this.$axios('/list.json').then(res=>{
             this.list =  res.data.result
         })
     }
 },
 computed: {
     newlist(){
        var arr = ''

        if(this.value == ''){
            return arr
        }

        arr = this.list.filter(item=>{
            return item.title.indexOf(this.value) != -1
        })

        
        return  arr
     },
     en(){
         console.log(this.value)
         this.ls.unshift(this.value)
         if(this.ls.length > 6){
             this.ls.pop()
         }
         this.value = ''
     }
 },
}
</script>

<style scoped lang="scss">
ul{
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    li{
        width: 15%;
        padding: 2%;
        box-sizing: border-box;
        margin: 5%;
        border: 1px solid black;
        border-radius: 15px;
        text-align: center;
    }
}
</style>
