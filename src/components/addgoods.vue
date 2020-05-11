<template>
  	<div>
		<!-- 头部组件 -->
		<myheader></myheader>

		<!-- 面包屑导航 -->
		<Breadcrumb :datas='datas'></Breadcrumb>

		<section class="featured-block text-center">
			<div class="container">
				<table>
					<tr>
						<td>
							商品名称：
						</td>
						<td>
							<input type="text" v-model="name" placeholder="请输入商品名称">
						</td>
					</tr>
					<tr>
						<td>
							商品价格：
						</td>
						<td>
							<input type="number" v-model="price">
						</td>
					</tr>
					<tr>
						<td>
							商品分类：
						</td>
						<td>
							<Select v-model="selected" :datas="category"></Select>
						</td>
					</tr>
					<tr>
						<td>
							商品颜色：
						</td>
						<td>
							<input type="text" v-model="color">
						</td>
					</tr>
					<tr>
						<td>
							商品尺码：
						</td>
						<td>
							<input type="text" v-model="size">
						</td>
					</tr>
					<tr>
						<td>
							季节：
						</td>
						<td>
							<input type="text" v-model="season">
						</td>
					</tr>
					<tr>
						<td>
							
						</td>
						<td>
							<Button @click="submit" color="green">添加商品</Button>
						</td>
					</tr>
				</table>
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
		datas: [{title: '首页', route: {name: 'index'}}, {title: '添加商品页'}],
		name: '',
		price: 0,
		color: '',
		size: '',
		season: '',
		// 商品分类
		category: [],
		// 分类默认选中
		selected: '衣服'
    }
  },

  // 注册组件标签
  components:{
	  'myheader': myheader,
	  myfooter
  },

  computed: {
	
  },

  mounted: function () {
	  this.get_category()
  },

  methods:{
	  // 获取商品分类
	  get_category: function () {
		  // 发送请求
		  this.axios.get('http://localhost:8000/categorylist').then((result) =>{
			  console.log(result)
			  var mycate = []
			  // 二次处理
			  for (let i=0; i<result.data.length; i++) {
				  mycate.push(result.data[i]['name'])
			  }
			  // 赋值
			  this.category = mycate
		  })
	  },

	  submit: function () {
		  // 将普通字段转换为json
		  var param = {};
		  param['color'] = this.color;
		  param['size'] = this.size;
		  param['season'] = this.season;
		  
		  console.log(param);

		  // 转换字符串
		  param = JSON.stringify(param);

		  console.log(param)

		  // 发送请求
		  this.axios.get('http://localhost:8000/insertgoods/', 
		  	{params: {
			  name: this.name,
			  price: this.price,
			  params: param
			}}).then((result) =>{
				console.log(result);
			this.$Message(result.data.message);
    	  });
	  }
  }
}
</script>
 
<style>
/* 标签选择器 */
td {
	padding: 5px;
}
</style>