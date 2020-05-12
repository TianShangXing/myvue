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
					
					<a href="#" class="btn btn-full-width btn-lg btn-outline-primary">Add to cart</a></div>
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
		mysrc: ''
    }
  },

  // 注册组件标签
  components:{
	  'myheader': myheader,
	  myfooter
  },

  mounted:function(){
	  //获取商品id
	  this.id = this.$route.query.id;

	  console.log(this.id);

	  this.get_good();
  },

  methods:{
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