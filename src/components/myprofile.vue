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
					<br>
					<Button color="red">删除</Button>
					&emsp;&emsp;
					<input type="text" /><Button color="blue">创建目录</Button>
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
						<td>
							七牛云上传：
						</td>
						<td>
							<input type="file" @change="upload_qiniu">
						</td>
					</tr>
					<tr>
						<td></td>
						<td>
							<video id="video" v-show="videosrc" width="300" height="200" :src="videosrc" controls="controls" autoplay="autoplay"></video>
							<br>
							<Button v-show="videosrc" @click="changepic" color='red'>{{ mybutton }}</Button>
						</td>
					</tr>
					<tr class="upload">
						<td>
							又拍云上传：
						</td>
						<td>
							<input type="file" @change="upload_upyun">
						</td>
					</tr>
					<tr>
						<td></td>
						<td>
							<video id="video" v-show="videosrc" width="300" height="200" :src="videosrc" controls="controls" autoplay="autoplay"></video>
							<br>
							<Button v-show="videosrc" @click="changepic" color='red'>{{ mybutton }}</Button>
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

// 导入文件
import {config, formatXml} from '../config'

export default {
  data () {
    return {
		// 面包屑导航
		datas: [{title: '首页', route: {name: 'index'}}, {title: '个人详情页'}],
		// 上传地址
		src:'',
		// 七牛云token
		token: '',
		// 视频地址
		videosrc: '',
		// 画中画切换按钮
	  	mybutton: '进入画中画',
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
	  this.get_token();
	
	  // 又拍云上传
	  // 注册拖拽容器
	  let upload = document.querySelector('.upload');
	  // 声明监听事件
	  upload.addEventListener('dragenter', this.onDrag, false);
	  upload.addEventListener('dragover', this.onDrag, false);
	  upload.addEventListener('drop', this.onDrop, false);
  },

  methods:{
	  onDrag (e) {
		e.stopPropagation();
		e.preventDefault();
	  },
	  
	  onDrop (e) {
		e.stopPropagation();
		e.preventDefault();
		// 调用自定义上传方法
		this.upload_upyun(e.dataTransfer.files);
	  },

	  // 上传又拍云
	  upload_upyun: function (files) {
		// 获取文件对象
		// let file = e.target.files[0];
		let file = files[0];

		// 声明参数
		let param = new FormData();
		param.append('file',file);

		const config = {
			headers: { 'Content-Type': 'multipart/form-data' }
		} 

		// 发送请求
		this.axios.post('http://localhost:8000/uploadup/', param, config)
		.then((result) => {
			console.log(result);
			this.src = 'http://tianshangxing.test.upcdn.net/test.jpg' 
		})
	  },

	  // 画中画切换
	  changepic: function () {
		  // 判断是否处于画中画界面
		  if (video !== document.pictureInPictureElement){
			  // 尝试进入画中画模式
			  video.requestPictureInPicture();
			  this.mybutton = '退出画中画';
		  } else {
			  // 退出画中画
			  document.exitPictureInPicture();
			  this.mybutton = '进入画中画';
		  }
	  },

	  get_token: function () {
		  // 请求后台接口 get -> params  post -> data
		  this.axios.get('http://127.0.0.1:8000/qiniu/').then((result) =>{
			  console.log(result);
			  this.token = result.data.token;
			  console.log(this.token)
		  })
	  },
	  
	  // 七牛云上传
	  upload_qiniu: function (e) {
		  // 获取文件
		  let file = e.target.files[0];

		  // 声明参数
		  let param = new FormData();
		  
		  //          文件key 文件实体 文件名称
		  param.append('file', file, file.name);
		  param.append('token', this.token);

		  // 自定义axios
		  const axios_qiniu = this.axios.create({withCredentials:false});
		  
		  // 发送请求
		  axios_qiniu({
			  method: 'POST',
			  url: 'http://up-z1.qiniup.com/',
			  data: param,
			  timeout: 30000
		  }).then(result => {
			  console.log(result);
			  this.src = config['baseurl'] + result.data.key;
			  this.videosrc = config['baseurl'] + result.data.key;
		  });
	  },

	  // 定义提交事件
	  upload: function (e) {
		  // 获取文件
		  let file = e.target.files[0];

		  // 声明参数
		  let param = new FormData();
		  
		  //          文件key 文件实体 文件名称
		  param.append('file', file, file.name);
		  param.append('uid', localStorage.getItem('uid'));

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
/* 类选择器 */
.upload {
  margin: 100px auto;
  width: 300px;
  height: 150px;
  border: 2px dashed #f00;
}

/* 标签选择器 */
td {
	padding: 5px;
}
</style>