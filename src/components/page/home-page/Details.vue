<template>
  <div class="Details">
    <header>
        <router-link tag="i" :to="{name:'HomePage'}" class="home-page">&lt;</router-link>
        图书详情
    </header>
    <!-- <div class="header"></div> -->
    <section>
        <!-- 图书信息 -->
        <div class="book-info">
          <!-- 图书头部信息 -->
          <div class="book-info-top">
           <img :src="book.cover" alt="">
            <span class="bit-content">
              <label for="">作者</label><br>
              <label for="">页数</label> <br> 
              <label for="">书号</label> <br> 
              <label for="">出版日期</label>                                
            </span>  
            <span class="bit-right">
              <label for="">{{book.author}}</label><br>
              <label for="">730</label><br> 
              <label for="">B00CBBJS5Y</label><br>  
              <label for="">2012年3月1日</label>  
            </span>
          </div>
          <!-- 书名 -->
          <p class="book-title">{{ book.name }}</p>  
        </div> 
      <div class="book-btn">
        <router-link tag="button" :to="{name:'ShoppingCart'}"><span @click="clickShopping">加入购物车</span></router-link>       
        <router-link tag="button" :to="{name:'ShoppingCart'}"><span @click="clickShopping">立即购买</span></router-link>       
        
        <!-- <button @click="clickShopping" >立即购买</button> -->
      </div>
      <div class="book-blank"></div>
      <div class="info-class">
        <fieldset>
          <legend>内容概要</legend>
          <div class="content-info">
            {{ book.describe}} 
            </div> 
        </fieldset> 
        <fieldset>
          <legend>内容概要</legend>
          <div class="content-info">
            {{ book.describe}} 
            </div> 
        </fieldset>
        <fieldset>
          <legend>作者简介</legend>
          <div class="content-info">
            {{ book.describe}} 
            </div> 
        </fieldset>
      </div>
    </section>
    <section class="mask" v-show="showmask">
        <div>
         <p>还没有登录，是否登录？</p>
         <router-link tag="button" 
         :to="{name:'PersonalPage'}"
         @click.native="maskbtn">确认</router-link>
        <button @click="maskbtn">取消</button>              
        </div> 
    </section>
  </div>
</template>

<script>
export default {
  name: 'Details',
  data () {
    let bookInfo = this.$route.params
    return {
      book:bookInfo,
      shoppingInfoS:[],
      showmask:false
    }
  },
  methods:{
    clickShopping(){
     
        let shoppingInfo = {
            "name":this.book.name,
            "author":this.book.author,
            "cover":this.book.cover,
            "price":this.book.price
        };
      this.$emit('shoppingInfo',shoppingInfo)
      // location.href='/shoppingcart'     
    
      },  
      maskbtn(){
        this.showmask=false
      }
  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import '../../common/common.scss';
@import './details.scss'
</style>