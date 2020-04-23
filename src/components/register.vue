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
							用户名：
						</td>
						<td>
							<input type="text" v-model="username" placeholder="请输入用户名">
						</td>
					</tr>
					<tr>
						<td>
							密码：
						</td>
						<td>
							<input type="password" v-model="password" placeholder="请输入密码">
						</td>
					</tr>
					<tr>
						<td>
							
						</td>
						<td>
							<Button color='blue' @click="submit">提交</Button>
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
		datas: [{title: '首页', route: {name: 'index'}}, {title: '注册页'}],
		// 用户名
		username: '',
		// 密码
		password: '',
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
	  
  },

  methods:{
	  // 定义提交事件
	  submit: function () {
		  // 非空验证
		  if (this.username == '') {
			  this.$Message('您没有输入用户名');
			  return false;
		  }
		  if (this.password == '') {
			  this.$Message('您没有输入密码');
			  return false;
		  }

		  // 请求后台接口 get -> params  post -> data
		  this.axios.get('http://localhost:8000/register/', {params:{
			  username: this.username,
			  password: this.password
		  }}).then((result) =>{
			  console.log(result);
			  this.$Message(result.data.message);
		  } )
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