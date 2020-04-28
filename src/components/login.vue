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
							验证码:						
						</td>
						<td>
							<input type="text" v-model="code" placeholder="请输入验证码">
						</td>
					</tr>
					<tr>
						<td></td>
						<td> 
							<img class="imgcode" alt="点击刷新" :src="src"  @click="changecode">
						</td>
					</tr>
					<tr>
						<td></td>
						<td> 
							<!--滑块验证码-->
							<drag-verify
							:width="width"
							:height="height"
							:text="text"
							ref="Verify"
							></drag-verify>
						</td>
					</tr>
					<tr>
						<td></td>
						<td>
							<Button color='green' @click="submit">登录</Button>
							&emsp;&emsp;
							<img class="imgcode" src="http://localhost:8000/static/sina.png" @click="sina">
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

//导入滑块验证码
import dragVerify from 'vue-drag-verify'; 

export default {
  data () {
    return {
		// 面包屑导航
		datas: [{title: '首页', route: {name: 'index'}}, {title: '登录页'}],
		// 用户名
		username: '',
		// 密码
		password: '',
		// 验证码图片地址
		src:'http://localhost:8000/code/',
		// 验证码
		code:'',
		// 声明滑块验证码相关数据
		width:320,
		height:42,
		text:'请将滑块拖动到右边',
    }
  },

  // 注册组件标签
  components:{
	  'myheader': myheader,
	  myfooter,
	  dragVerify
  },

  computed: {
	
  },

  mounted: function () {
	  
  },

  methods:{
	  // 新浪微博三方登录
	  sina: function () {
		// 拼接url
		let client_id = 1484610767;
		
		let url = 'https://api.weibo.com/oauth2/authorize?client_id=' + client_id + '&redirect_uri=http://127.0.0.1:8000/md_admin/weibo';

		// 跳转 站外跳转：window.location.href
		window.location.href = url;
	  },

	  // 刷新验证码
  	  changecode:function () {
  		// 生成随机数
  		var num = Math.ceil(Math.random()*10);

  		// 进行传参
  		this.src = this.src + "?num=" + num;
	  },
	  
	  // 定义提交事件
	  submit: function () {
		  //判断是否拖动
  		  console.log(this.$refs.Verify.isPassing);
		  // 非空验证
		  if (this.username == '') {
			  this.$Message('您没有输入用户名');
			  return false;
		  }
		  if (this.password == '') {
			  this.$Message('您没有输入密码');
			  return false;
		  }

		  if (this.code == '') {
			  this.$Message('您没有输入验证码');
			  return false;
		  }

		  // 判断是否拖动滑块验证
          if (this.$refs.Verify.isPassing == false){
              this.$Message('请先拖动滑块');
              return false;
		  }

		  // 请求后台接口 get -> params  post -> data
		  this.axios.get('http://localhost:8000/login/', {params:{
			  username: this.username,
			  password: this.password,
			  code: this.code
		  }}).then((result) =>{
			  console.log(result);
			  if (result.data.code == 200) {
				// 登录成功
				localStorage.setItem('username', result.data.username);
				localStorage.setItem('uid', result.data.uid);
				this.$Message(result.data.message);
				// 跳转页面
				this.$router.push('/')
			  } else {
				this.$Notice(result.data.message);
			  }
			  
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
.imgcode {
	cursor:pointer;
}
</style>