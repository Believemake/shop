<template>
	<view class="login">
		
		<view class="logo">
			<image src="../../../static/image/logo.png" mode=""></image>
		</view>
		<view class="login-center">
			<view>
				<image class="icon-sh" :src="$websiteUrl('/upload/uniapp/login/login_zu.png')" mode=""></image>
				<input type="number" placeholder="输入手机号" @input="Phone" />
			</view>
			<view>
				<image class="icon-sh" :src="$websiteUrl('/upload/uniapp/login/login_zu3.png')" mode=""></image>
				<input :type="isflag==true?'password':'text'" placeholder="输入密码" @input="Pwd" />
				<view @tap.stop="isBlock">
					<image class="icon-sh" v-show="isblock" :src="$websiteUrl('/upload/uniapp/login/login_password.png')" mode="" ></image>
					<image class="icon-sh" v-show="!isblock" :src="$websiteUrl('/upload/uniapp/login/login_wx_20190731145902.png')" mode="" ></image>
				</view>
			</view>
			<view class="argin">
				<text @tap.stop='register(1)'>立即注册</text> <text @tap.stop='register(2)'>找回密码</text>
			</view>
		</view>
		
		<view class="but" @tap.stop="Login">
			立即登录
		</view>
	</view>
</template>

<script>
	import {postApi} from '@/common/api/api.js'
	import {url} from '@/common/api/url.js'
	import {navGo}  from "@/common/nav/nav.js"
	export default {
		data() {
			return {
				phone:0,
				password:'',
				isblock:true,
				isflag:true
			}
		},
		methods: {
			isBlock(){ //控制密码显示隐藏
				this.isblock = this.isblock == true ? false : true;
				this.isflag = this.isflag == true ? false : true;
			},
			Phone(e){ //获取手机号
				this.phone=e.detail.value;
			},
			Pwd(e){ //获取密码
				this.password=e.detail.value;
				console.log(e)
				if(e.detail.value==''){
					this.isblock=true;
				}
			},
			register(number){
				switch(number){
					case 1:
					navGo('/pages/public/register/register');
					break;
					case 2:
					navGo('/pages/public/find/find');
					break;
				}
			},
		async Login(){ //立即登录
			   await postApi(url.login,{
				   data: {
				   	mobile: this.phone,
				   	password: this.password
				   }
			   }).then(res=>{
				   this.$msg(res.data.msg);
				   if(res.data.code==1){
				    uni.setStorageSync('token', res.data.data.token);
					  uni.switchTab({
					  	url:'../../index/index'
					  })
				   }
			   })
			}
		}
	}
</script>

<style lang="scss">
	page{
		width: 100%;
		height: 100%;
		 font-size:30rpx;
	}
	.login{
		display: flex;
		// justify-content: center;
		align-items: center;
		flex-direction: column;
		
	 .logo{
		 margin-top:200rpx;
		 image{
			 width: 120rpx;
			 height: 120rpx;
		 }
	 }
	 .login-center{
		 width: 500rpx;
		 margin-top: 300rpx;
		 .icon-sh{
			 width: 48rpx;
			 height: 60rpx;
		 }
		 view{
			 height: 100rpx;
			 display: flex;
			 align-items: center;
			 border-bottom: 1rpx solid #eee;
			 input{
				 margin-left: 30rpx;
			 }
		 }
		 .argin{
			 display: flex;
			 align-items: center;
			 justify-content: space-between;
			 color: #13227A;
			 font-size:30rpx;
			 border-bottom: none;
		 }
	 }
	 .but{
		 background-color:$font-color1;
		 border-radius: 40rpx;
		 height: 80rpx;
		 width: 400rpx;
		 color:$uni-text-color-inverse;
		 display: flex;
		 justify-content: center;
		 align-items: center;
		 margin-top: 100rpx;
	 }
	}
</style>
