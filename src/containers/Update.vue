<template>
    <div>
      <m-header title="修改页面"></m-header>
      <div class="add">
        <div class="group">
          <label for="bookName">美的定义</label>
          <input type="text" id="bookName" placeholder="请输入名字" v-model="book.bookName">
        </div>
        <div class="group">
          <label for="bookName">照片地址</label>
          <input type="text" id="bookCover" placeholder="请输入照片的路径" v-model="book.bookCover">
        </div>
        <div class="group">
          <label for="content">关于美的描述</label>
          <input type="text" id="content" placeholder="请输入详细内容" v-model="book.content">
        </div>
        <div class="group">
          <button @click="update">修改图片</button>
          <router-link to="/list" tag="button">返回</router-link>
        </div>
      </div>
    </div>
</template>
<script>
    import MHeader from 'components/MHeader';
    import {getOneBook,updateBook} from 'api';
    export default {
        data(){
            return {
                book:{
                    bookName:'',
                    bookCover:'',
                    content:''
                }
            }
        },
        created(){
            //id是当前路径的参数id
            this.getBook();
        },
        computed: {},
        components: {MHeader},
        methods: {
            getBook(){
              getOneBook(this.$route.params.id).then(res=>{
                this.book = res.data;//将数据挂载视图上
              });
            },
            update(){
              updateBook(this.$route.params.id,this.book).then(()=>{
                  this.$router.push('/list')
              });
            }
        },
        activated(){
            this.getBook();
        }
    }
</script>
<style scoped lang="less">
  .add{
    padding: 0 20px;
    display: flex;
    flex-direction: column;
    .group{
      label{
        font-size: 16px;
        line-height: 25px;
      }
      input{
        border: 1px solid #ccc;
        height: 30px;
      }
      margin-bottom: 10px;
      display: flex;
      flex-direction: column;
      button{
        height: 35px;
      }
    }
  }
</style>
