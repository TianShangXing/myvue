<template>
  	<div>
	  	<!-- 头部组件 -->
		<myheader></myheader>

		<!-- 面包屑导航 -->
		<Breadcrumb :datas='datas'></Breadcrumb>
		
		<section class="products text-center">
			<div class="container">
				<h3 class="mb-4">商品检索</h3>
				<div class="row">
					<div v-for="item in goodslist" class="col-sm-6 col-md-3 col-product">
						<figure>
							<img class="rounded-corners img-fluid" :src="'http://localhost:8000/static/upload/' + item.img"	width="240" height="240">
							<!-- <video class="rounded-corners img-fluid" :src="'http://localhost:8000/static/upload/' + item.video" width="240" height="240" autoplay="autoplay" controls="controls"></video> -->
							<figcaption>
								<div class="thumb-overlay"><a href="item.html" title="More Info">
									<i class="fas fa-search-plus"></i>
								</a></div>
							</figcaption>
						</figure>
						<h4><a :href="'http://localhost:8080/item?id=' + item.id"><div v-html="$options.filters.myfilter(item.name)"></div></a></h4>
						<p><span class="emphasis">${{ item.price }}</span></p>
					</div>
				</div>

				<!-- HeyUI分页逻辑 -->
				<div>
					<Pagination v-model="pagination" @change="get_goods" align="center" layout="pager, jumper" small></Pagination>
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
	  datas: [{title: '首页', route: {name: 'index'}}, {title: '商品检索页'}],
	  // 商品列表
	  goodslist: [],
	  // 分页器变量
	  pagination: {
		  // 当前页
		  page: 1,
		  // 每页展示多少个
		  size: 2,
		  // 总数
		  total: 5
	  },
	  // 搜索关键词
	  text: ''
    }
  },
  
  // 注册组件标签
  components:{
	  'myheader': myheader,
	  myfooter
  },

  mounted:function(){
	  // 传递公共变量
	  window.that = this;

	  // 接收参数
	  var text = this.$route.query.text;
	  console.log(text);

	  // 判断
	  if (text.indexOf(' ')) {
		  text = text.split(" ");

		  // 格式转换
		  text = JSON.stringify(text);
	  }
	  this.text = text;

	  console.log(text);

	  this.get_goods();
  },

  // 自定义过滤器
  filters: {
	  myfilter: function (value) {
		  console.log(window.that.text);

		//   value = value.replace(new RegExp(window.that.text, 'g'), '<span class="highlight">' + window.that.text + '</span>');

		  return value;
	  }
  },

  methods:{
	  // 获取商品
	  get_goods: function () {
		  // 发送请求
		  this.axios.get('http://localhost:8000/search/',{params: {
			  page: this.pagination.page,
			  size: this.pagination.size,
			  text: this.text
		  }}).then((result) => {
			  console.log(result);
			  
			//   for (let i=0; i<result.data.data.length; i++) {
			// 	  result.data.data[i]['name'] = result.data.data[i]['name'].replace(new RegExp(this.text, 'g'), '<span class="highlight">' + this.text + '</span>')
			//   }
			  console.log(result.data.data)

			  this.goodslist = result.data.data;

        	  this.pagination.total = result.data.total;
		  })
	  }
  }
}


</script>
 
<style>
.highlight{
  color:red;
}
</style>