<template>
    <div>
      <m-header title="列表页"></m-header>
      <scroller :on-refresh="refresh" ref="scroller" class="top">
      <ul class="list">
        <li v-for="book in books">
          <img v-lazy="book.bookCover" alt="">
          <div>
            <h3>{{book.bookName}}</h3>
            <p>{{book.content}}</p>
            <i class="iconfont icon-shoucang" @click="save(book)"></i>
            <p class="btn-list">
              <router-link :to="{path:'/update/'+book.id}" tag="button">修改</router-link>
              <button @click="remove(book.id)">删除</button>
            </p>
          </div>
        </li>
      </ul>
      </scroller>
    </div>
</template>
<script>
    import {mapMutations} from 'vuex';
    import MHeader from 'components/MHeader';
    import * as Types from '../store/mutation-types'
    import {getBook,removeBook} from 'api';
    export default {
        data(){
            return {books:[]}
        },
        created(){
          this.getList();
        },
        computed: {},
        components: {MHeader},
        methods: {
            ...mapMutations([Types.ADD_COLLECT]),
            save(book){
              this[Types.ADD_COLLECT](book);
              this.$router.push('/collect');
            },
            remove(id){
              removeBook(id).then(res=>{
                this.books = this.books.filter(item=>item.id!=id);
              });
            },
            refresh(){
              this.getList();
            },
            getList(){
              getBook().then(res=>{
                this.books = res.data;
                this.$refs.scroller.finishPullToRefresh();
              });
            }
        }
    }
</script>
<style scoped lang="less">
  .btn-list{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    button{
      width: 60px;
    }
  }
.list{
  li{
    display: flex;
    border-bottom: 1px solid #ccc;
    img{width: 100px;height: 90px;margin: 10px}
    div{
      h3{color: #666666;margin: 5px;}
      display: flex;
      flex-direction: column;
      width:150px;
    }
  }
  img[lazy="loading"]{
    background: url("../assets/loading.gif") no-repeat center;
    background-size: 50%;
  }
  img[lazy="loaded"]{
    animation: fadeIn 0.5s;
  }
}
  @keyframes fadeIn {
    from{opacity: 0}
    to{opacity: 1}
  }
.top{margin-top:40px}
  .icon-shoucang{font-size: 22px;color: orangered}
</style>
