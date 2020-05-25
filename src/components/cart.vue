<template>
  <div>
    <!-- 头部组件 -->
	<myheader></myheader>

	<!-- 面包屑导航 -->
	<Breadcrumb :datas='datas'></Breadcrumb>

    <section class="cart text-center">
      <div class="container">
        <div class="row">
          <div class="col-sm-6 mb-3 mb-m-1 text-md-left">
            <a href="catalog.html">
              <i class="fas fa-arrow-left mr-2"></i> Continue Shopping
            </a>
          </div>
          <div class="col-sm-6 text-md-right">
            <a href="catalog.html" class="btn btn-primary btn-lg pl-5 pr-5">Checkout</a>
          </div>
        </div>
        <div class="row">
          <div class="col-12 text-center">
            <h2 class="mt-5 mb-2">Your Shopping Cart</h2>
            <p class="mb-5">
              <span class="primary-color">3</span> Items in your cart
            </p>
            <table id="cart" class="table table-condensed">
              <thead>
                <tr>
                  <th style="width:60%">Product</th>
                  <th style="width:12%">Price</th>
                  <th style="width:10%">Quantity</th>
                  <th style="width:16%"></th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in cartlist">
                  <td data-th="Product">
                    <div class="row">
                      <div class="col-md-3 text-left">
                        <img :src="'http://localhost:8000/static/upload/'+item.img" alt class="img-fluid" />
                      </div>
                      <div class="col-md-9 text-left mt-sm-2">
                        <h4>{{ item.name }}</h4>
                        <p>York &amp; Smith</p>
                      </div>
                    </div>
                  </td>
                  <td data-th="Price">${{ item.price }}</td>
                  <td data-th="Quantity">
                    <input type="number" class="form-control text-center" v-model="item.num">
                  </td>
                  <td class="actions" data-th>
                    <div class="text-right">
                      <button class="btn btn-white btn-md mb-2">
                        <i class="fas fa-sync"></i>
                      </button>
                      <button class="btn btn-white btn-md mb-2">
                        <i class="fas fa-trash"></i>
                      </button>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
            <div class="float-right text-right">
              <h4>Subtotal:</h4>
              <h1>$147.00</h1>
              <p>(Excluding Delivery)</p>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-sm-6 mb-3 mb-m-1 text-md-left">
            <a href="catalog.html">
              <i class="fas fa-arrow-left mr-2"></i> Continue Shopping
            </a>
          </div>
          <div class="col-sm-6 text-md-right">
            <a href="catalog.html" class="btn btn-primary btn-lg pl-5 pr-5">Checkout</a>
          </div>
        </div>
      </div>
    </section>

    <!-- 尾部组件 -->
	<myfooter></myfooter>
  </div>
</template>


 
<script>
// 导入组件
import myheader from './myheader'
import myfooter from './myfooter'
export default {
  data() {
    return {
	  msg: "这是一个变量",
	  // 面包屑导航
	  datas: [{title: '首页', route: {name: 'index'}}, {title: '购物车页'}],
	  cartlist: []
    };
  },

  // 注册组件标签
  components:{
	  'myheader': myheader,
	  myfooter
  },

  mounted: function() {
	  this.init_cart();
  },

  methods: {
	  //初始化购物车
  	  init_cart:function(){
  		var cartdata = localStorage.getItem("cart");

  		if (cartdata == null) {
  			this.cartlist = [];
  		} else {
  			this.cartlist = JSON.parse(cartdata);
  		}
  	}
  }
};
</script>
 
<style>
</style>