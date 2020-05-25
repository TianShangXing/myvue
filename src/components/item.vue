<template>
  <div>
	<!-- 头部组件 -->
	<myheader></myheader>

	<!-- 面包屑导航 -->
	<Breadcrumb :datas='datas'></Breadcrumb>

	<section class="featured-block text-center">
		<div class="container">
			<div class="row">
				<div class="col-md-6 text-center">
					<div class="product-image mt-3">
						<img class="img-fluid" :src="mysrc">
					</div>
					<div class="product-thumbnails">
						<img @click="changeimg('placeholder-product.1011f1b.jpg')" class="mt-2 mr-2 img-fluid" src="../assets/images/placeholder-product.jpg">
						
							<img class="mt-2 mr-2 img-fluid" src="../assets/images/placeholder-product.jpg">
						
							<img class="mt-2 mr-2 img-fluid" src="../assets/images/placeholder-product.jpg">
						
							<img class="mt-2 mr-2 img-fluid" src="../assets/images/placeholder-product.jpg">
						
						<img class="mt-2 mr-2 img-fluid" src="../assets/images/placeholder-product.jpg">
					</div>
				</div>
				<div class="col-md-6 mt-5 mt-md-2 text-center text-md-left">
					<h2 class="mb-3 mt-0">{{ info.name}}</h2>
					<p class="lead mt-2 mb-3 primary-color">${{ info.price }}</p>
					{{ info.params }}
					<br>
					颜色：{{ param.color }}
					<br>
					号码：{{ param.size }}
					<br>
					<h5 class="mt-4">Description</h5>
					<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus venenatis velit vestibulum massa sollicitudin auctor. Cras ac venenatis orci. Ut consequat, purus ut ultrices ultricies, nisi sem ornare quam, sed ultricies mi nisl sit amet purus.</p>
					<p>Suspendisse potenti. Nunc in libero luctus, sagittis leo sit amet, volutpat lacus. Quisque a porta risus. Integer aliquet nibh vitae vestibulum accumsan</p>
					<h5 class="mt-5">Care Instructions</h5>
					<p>Suspendisse cursus erat sed sem sagittis cursus. Etiam porta sem malesuada magna mollis euismod.</p>
					<select class="custom-select form-control mt-4 mb-4">
						<option selected>Size</option>
						<option value="1">Small</option>
						<option value="2">Medium</option>
						<option value="3">Large</option>
					</select>
					
					<!--Quantity: <input type="text" class="form-control quantity mb-4" name="" value="1">-->
					
					<a href="javascript:void(0)" @click="add_cart" class="btn btn-full-width btn-lg btn-outline-primary">Add to cart</a>
					
					<br><br>

					<!-- 商品评论 -->

					<textarea v-model="comment" rows="10" v-autosize v-wordcount="100">
						

					</textarea>

					<br><br>

					<Button @click="submit" color="blue">提交评论</Button>

					<!-- 评论列表 -->
					<ul>	
						<li v-for="item in commentlist">

						{{ change_uid(item.uid) }} 说:{{ item.content }}

						</li>
					</ul>
				</div>
			</div>
		</div>
	</section>
	
	<div class="divider"></div>
	
	<section class="products text-center">
		<div class="container">
			<h3 class="mb-4">Related Products</h3>
			<div class="row">
				<div class="col-sm-6 col-md-3 col-product">
					<figure>
						<img class="rounded-corners img-fluid" src="../assets/images/placeholder-product.jpg"	width="240" height="240">
						<figcaption>
							<div class="thumb-overlay"><a href="#" title="More Info">
								<i class="fas fa-search-plus"></i>
							</a></div>
						</figcaption>
					</figure>
					<h4><a href="#">Product Name</a></h4>
					<p><span class="emphasis">$19.00</span></p>
				</div>
				<div class="col-sm-6 col-md-3 col-product">
					<figure>
						<img class="rounded-corners img-fluid" src="../assets/images/placeholder-product.jpg"	width="240" height="240">
						<figcaption>
							<div class="thumb-overlay"><a href="#" title="More Info">
								<i class="fas fa-search-plus"></i>
							</a></div>
						</figcaption>
					</figure>
					<h4><a href="#">Product Name</a></h4>
					<p><span class="emphasis">$19.00</span></p>
				</div>
				<div class="col-sm-6 col-md-3 col-product">
					<figure>
						<img class="rounded-corners img-fluid" src="../assets/images/placeholder-product.jpg"	width="240" height="240">
						<figcaption>
							<div class="thumb-overlay"><a href="#" title="More Info">
								<i class="fas fa-search-plus"></i>
							</a></div>
						</figcaption>
					</figure>
					<h4><a href="#">Product Name</a></h4>
					<p><span class="emphasis">$19.00</span></p>
				</div>
				<div class="col-sm-6 col-md-3 col-product">
					<figure>
						<img class="rounded-corners img-fluid" src="../assets/images/placeholder-product.jpg"	width="240" height="240">
						<figcaption>
							<div class="thumb-overlay"><a href="#" title="More Info">
								<i class="fas fa-search-plus"></i>
							</a></div>
						</figcaption>
					</figure>
					<h4><a href="#">Product Name</a></h4>
					<p><span class="emphasis">$19.00</span></p>
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
  data () {
    return {
      	// 面包屑导航
		datas: [{title: '首页', route: {name: 'index'}}, {title: '商品详情页'}],
		// 商品id
		id: "",
		// 商品详情
		info: {},
		// 商品规格
		param: {},
		// 图片地址
		mysrc: '',
		// 评论内容
		comment:'',
		// 评论列表
		commentlist:[],
		// 用户名列表
		users: {},
		// 购物车列表
		cartlist: []
    }
  },

  // 注册组件标签
  components:{
	  'myheader': myheader,
	  myfooter
  },

  mounted:function(){
	  // 获取商品id
	  this.id = this.$route.query.id;

	//   console.log(this.id);

	  // 获取商品信息
	  this.get_good();

	  // 获取商品评论
	  this.get_comments();

	  // 获取用户名列表
	  this.get_users();
		
	  // 获取购物车列表
	  this.init_cart();
  },

  methods:{
	  // 初始化购物车
	  init_cart: function () {
		  var cartdata = localStorage.getItem('cart')
		  
		  if (cartdata == null) {
			  this.cartlist = [];
		  } else {
			  this.cartlist = JSON.parse(cartdata)
		  }
	  },
	  // 添加购物车
	  add_cart: function () {
		  // 默认购物车内没有商品
		  let findgood = 0;

		  for(let i=0; i<this.cartlist.length; i++) {
			  if (this.info.name == this.cartlist[i]['name']) {
				  this.cartlist[i]['num'] ++;
				  // 声明找到了该商品
				  findgood = 1;
				  // 结束循环
				  break;
			  }
		  }

		  // 如果该商品没有在购物车
		  if(findgood == 0) {
			  this.cartlist.push({'name': this.info.name, 'price': this.info.price, 'num': 1, 'img': this.info.img});
		  }

		  console.log(this.cartlist)

		  // 状态保持
		  localStorage.setItem('cart', JSON.stringify(this.cartlist))
	  },

	  // 替换用户名
  	  change_uid: function (uid) {

  		return this.users[uid];

	  },
	  
  	  // 获取用户名
  	  get_users: function () {

  	  //发送请求
      this.axios.get('http://localhost:8000/getusers/').then((result) =>{

        console.log(result);

        //遍历
        for(let i=0; i<result.data.length; i++){

        	this.users[result.data[i]['id']] = result.data[i]['username'];

		}
		
		console.log(this.users);
		
      	});
	  },
	  
	  //获取商品评论
  	  get_comments:function(){
  		//发送请求
     	this.axios.get('http://localhost:8000/commentlist/', {params: {
			gid:this.id
		}}).then((result) =>{

        	console.log(result);

		this.commentlist = result.data;
		
      	});
	  },
	  
	  // 提交评论
  	  submit: function () {
  		if (this.comment == '') {
  			this.$Message("评论不能为空");
  			return false;
  		}

  		this.comment = this.comment.replace(/ /g,'');

  		if (this.comment.length > 100) {
  			this.$Message("超出了长度限制");
  			return false;
  		}

  		//请求入库
  		//发送请求
		this.axios.post('http://localhost:8000/commentinsert/',this.qs.stringify({
		  uid:localStorage.getItem("uid"),
		  gid:this.id,
		  content:this.comment
		})).then((result) =>{
        	console.log(result);
		this.$Message(result.data.message);
		
		// this.get_comments();

        // 添加评论数据
        this.commentlist.unshift({'uid':localStorage.getItem("uid"),'content':this.comment});
      	});
  	  },

	  // 更换大图
	  changeimg: function (img) {
		  console.log(img);
		  this.mysrc = 'http://localhost:8080/static/img/' + img;
	  },

	  // 获取商品详情
	  get_good: function () {
      // 发送请求
      this.axios.get('http://localhost:8000/goodinfo/', {
		params: {
			id:this.id
		}}).then((result) =>{
			console.log(result);

		this.info = result.data;

		// 给图片地址赋值
        this.mysrc = 'http://localhost:8000/static/upload/'+ this.info.img;
			
        // 类型转换
        this.param = JSON.parse(this.info.params);

      	});

      },
     
  }
}


</script>

<style>
.img-fluid{
	cursor:pointer;
}
</style>