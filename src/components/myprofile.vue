<template>
  	<div>
		<!-- 头部组件 -->
		<myheader></myheader>

		<!-- 面包屑导航 -->
		<Breadcrumb :datas='datas'></Breadcrumb>

		<section class="featured-block text-center">
			<div class="container">
				<div>
					<Avatar :src="src" :width="200" fit="fill"></Avatar>
				</div>
				<table>
					<tr>
						<td>
							用户头像：
						</td>
						<td>
							<input type="file" @change="upload">
						</td>
					</tr>
					<tr>
						<td></td>
						<td>
							<!-- <Button color='green' @click="submit">上传</Button> -->
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
		datas: [{title: '首页', route: {name: 'index'}}, {title: '个人详情页'}],
		// 上传地址
		src:'',
    }
  },

  // 注册组件标签
  components:{
	  'myheader': myheader,
	  myfooter,
  },

  computed: {
	
  },

  mounted: function () {
	  
  },

  methods:{
	  // 定义提交事件
	  upload: function (e) {
		  // 获取文件
		  let file = e.target.files[0];

		  // 声明参数
		  let param = new FormData();
		  
		  //          文件key 文件实体 文件名称
		  param.append('file', file, file.name)

		  // 声明请求头
		  let config = {headers: {'Content-Type': 'multipart/form-data'}}

		  // 请求后台接口 get -> params  post -> data
		  this.axios.post('http://127.0.0.1:8000/upload/', param, config)
		  .then((result) =>{
			  console.log(result);
			  this.src = 'http://127.0.0.1:8000/static/upload/' + result.data.filename;
		  })
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