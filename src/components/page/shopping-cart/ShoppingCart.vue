<template>
  <div class="ShoppingCart">
   <section class="shoppingcart">
      <header>
      购物车
      </header>
    <div class="header"></div>
    <div class="shopping-blank" v-show="shoppingshow">
      <span class="shopping-blank-span">
        购物车里空空如也，<router-link tag="a" :to="{name:'HomePage'}">去逛逛？</router-link>
      </span>
    </div>
    <div class="commodity" v-show="aaa">
      <!-- 编辑和删除部分 -->
    <div class="cart-delete">
      <span v-if="eddit" class="cart-eddit" @click="edditBtn">编辑</span>
      <div v-else class="cart-dele">
        <span class="cart-cancle" @click="canclebtn">取消</span>
        <span class="cart-Selected" @click="deletebtn">删除选中</span>
      </div>
    </div>
    <ul>
      <li v-for="(book,idx) in cartBookList" :key="book.id">
        <!-- 勾选框 -->
        <div class="cart-box">
          <i class="cart-not-select" v-if="book.notSelect" @click="showSelect(idx)">
            <span class="cart-span"></span>
            </i>
          <i class="cart-select" v-if="book.select" @click="showNotSelect(idx)">
            <span class="cart-span-c">✓</span>
          </i>
        </div>
        <div class="cart-book-box" :class="{moveBookBox: book.moveBookBox}">
          <img :src="book.cover" alt="" class="cart-img">
          <div class="clear"></div>
          <div class="infoBox">
            <p class="bookname">{{book.name}}</p>
            <p class="bookauthor">{{book.author}}</p>
            <p  class="bookprice">¥&nbsp;&nbsp;{{book.price}}</p>
          </div>
          <div class="cart-count-box" v-if="book.showCount">
            <span class="cart-min"  @click="cartMin">-</span>
            <span class="cart-count">{{book.cartCount}}</span>
            <span class="cart-add"  @click="cartAdd">+</span>
          </div>
        </div>
      </li>
    </ul>
    <div class="cart-pay">
      <button @click="showcenter" class="btn"> 前往支付</button>
    </div>
    </div>
    <div class="userInfo" v-show="userInfomrsk">
      <div class="userInfomask">
        <span>您的收货地址不完善，是否完善？</span><br>
        <router-link tag="button" :to="{name:'PersonalPage'}" class="useryes">是</router-link>
        <button class="userno" @click="closecenter">否</button>
      </div>
    </div>
   </section>
  </div>
</template>

<script>

export default {
  name: 'ShoppingCart',
  data () {
    return {
     cartBookList:[],
       show:false,
       eddit:true,
       money:NaN,
       totalprice:null,
       shoppingshow:false,
       aaa:false,
       userInfomrsk:false
    }
  },
    components:{
    },
     methods: {
      // 从本地存储获取到用户已经点击加入过购物车的书本
        getLocalBookList() {
            if(!localStorage.getItem("shoppingInfos")){
               this.shoppingshow=true;
                return;
            }else{
                // 从本地取出书的数组
                 this.aaa=true;
                this.shoppingshow = false;
                let localBookArr = JSON.parse(localStorage.getItem("shoppingInfos")),
                    localBookArr_len = localBookArr.length;
                for(let i = 0;i < localBookArr_len;i++){
                    localBookArr[i].notSelect = false;
                    localBookArr[i].select = false;
                    localBookArr[i].moveBookBox = false;
                    localBookArr[i].showCount = true;
                    localBookArr[i].cartCount = 1;
                }
                this.cartBookList = localBookArr;
            }
        },
        //点击编辑按钮
        edditBtn(){
          this.eddit=false;
          let cartBookList_len = this.cartBookList.length;
            for(let i = 0;i < cartBookList_len;i++){
                this.cartBookList[i].select = false;
                this.cartBookList[i].notSelect = true;
                this.cartBookList[i].moveBookBox = true;
                this.cartBookList[i].showCount = false;
            }
        },
        // 点击取消
        canclebtn() {
            this.eddit = true;
            // 隐藏勾选框、恢复图书信息、显示计数框
            let cartBookList_len = this.cartBookList.length;
            for(let i = 0;i < cartBookList_len;i++){
                this.cartBookList[i].select = false;
                this.cartBookList[i].notSelect = false;
                this.cartBookList[i].moveBookBox = false;
                this.cartBookList[i].showCount = true;
            }
        },
        // 点击删除
        deletebtn() {
            // 如果当前还存在数据
            if(this.cartBookList){
                // 找出此时被选中的图书信息
                for(let i = 0;i < this.cartBookList.length;i++){
                    if(this.cartBookList[i].select === true){
                        // 删除本条数据
                        this.cartBookList.splice(i,1);
                        i--;
                    }
                }
            }  
        },
        // 点击不勾选书本
        showNotSelect(idx) {
              this.cartBookList[idx].select = false;
              this.cartBookList[idx].notSelect = true;
        },
        // 点击勾选书本
        showSelect(idx) {
              this.cartBookList[idx].select = true;
              this.cartBookList[idx].notSelect = false;
        },
        // 点击减少书本
        cartMin() {
          let cartBookList_len = this.cartBookList.length;
            for(let i = 0;i < cartBookList_len;i++){
              if(this.cartBookList[i].cartCount > 1){
                this.cartBookList[i].cartCount--;
            }
              else{
                  return
              } 
            }
        },
        // 点击增加书本
        cartAdd() {
          let cartBookList_len = this.cartBookList.length;
          for(let i = 0;i < cartBookList_len;i++){
              this.cartBookList[i].cartCount++;
            }
        },
        //收货地址显隐及总价计算
      showcenter(){
        let userInfo = localStorage.getItem("useraddress")
        if(!userInfo){
          this.userInfomrsk = true
        }else{
          let cartBookList_len = this.cartBookList.length;
          var num=0;
             for(let i = 0;i < cartBookList_len;i++){
                num+=this.cartBookList[i].cartCount*this.cartBookList[i].price
             }
          this.money =num;
          let shoppingprice = this.money.toFixed(2);
          localStorage.setItem("price",shoppingprice);
          location.href='/settlement'
        }
      
      },
      //收货地址显隐
      closecenter(){
        this.userInfomrsk=false
      },
     
    },
    created() {
        this.getLocalBookList();
    },
    computed:{
      totalPrice(){
       
      }
    }

   
}


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 @import '../../common/common.scss';
 @import './ShoppingCart.scss';
</style>
