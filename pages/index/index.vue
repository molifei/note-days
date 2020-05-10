<template>
	<view class="content">
		<view class="todo-header" v-if="list.length !== 0">
			<!-- 状态栏的左侧 -->
			<view class="tode-header_left">
				<text class="active-text">{{ text }}</text>
				<text>{{ listData.length }}条</text>
			</view>
			<!-- 右侧 -->
			<view class="tode-header_right">
				<text class="todo-header_right-item" :class="{ 'active-tab': activedIndex === 0 }" @click="tab(0)">全部</text>
				<text class="todo-header_right-item" :class="{ 'active-tab': activedIndex === 1 }" @click="tab(1)">待办</text>
				<text class="todo-header_right-item" :class="{ 'active-tab': activedIndex === 2 }" @click="tab(2)">已完成</text>
			</view>
		</view>

		<!-- 如果没有数据，显示此项 -->
		<view class="noList" v-if="list.length === 0">
			<view class="iconfont icon-daibanshixiang"></view>
			<view class="tip-text">
				<view>当前没有待办事项哦</view>
				<text>点击下方＋号创建一个吧</text>
			</view>
		</view>

		<view class="todo-content" v-else>
			<view class="todo-list" :class="{ 'todo--finish': item.checked }" v-for="(item, index) in listData" :key="item.id">
				<view class="todo-list_checkbox" @click="finish(item.id)"><view class="checkbox"></view></view>
				<view class="todo-list_content">{{ item.content }}</view>
			</view>
		</view>

		<!-- 创建按钮 -->
		<text :class="['create-todo', 'iconfont', 'icon-add', { 'rotate': animate }]" @click="createShow()"></text>
		<!-- 创建的元素 -->
		<view class="create-content" :class="{'create-show':animate}" v-if="textShow">
			<div class="create-content-box">
				<view class="create-input"><textarea type="text" value="" placeholder="输入事项" v-model="value" /></view>
				<view :class="['create-button', { 'btn-color': value }]" @click="created()">创建</view>
			</div>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			// 输入框的文字
			value: '',
			// 待办事项数组
			list: [
				
			],
			// 输入框是否显示
			textShow: false,
			// 初始显示状态
			activedIndex: 0,
			// 当前选中状态
			text: '全部',
			// 动画添加
			animate: false
		};
	},
	onLoad() {
		console.log(this.value);
	},
	computed: {
		listData() {
			// 深拷贝一个数据
			let list = JSON.parse(JSON.stringify(this.list));
			if (this.activedIndex === 0) {
				//全部
				return list;
			}
			if (this.activedIndex === 1) {
				// 待办事项
				this.text = '待办';
				return list.filter(item => item.checked === false);
			}
			if (this.activedIndex === 2) {
				// 已完成事项
				this.text = '已完成';
				return list.filter(item => item.checked === true);
			}
		}
	},
	methods: {
		// 点击+号
		createShow() {
			if(this.textShow){
				this.close()
			}else{
				this.open()
			}
		},
		// 打开动画
		open(){
			this.textShow = true;
			this.$nextTick(()=>{
				setTimeout(()=>{this.animate = true},50)
			})
		},
		// 关闭动画
		close(){
			this.animate = false;
			this.$nextTick(()=>{
				setTimeout(()=>{this.textShow = false},350)
			})
		},
		// 点击创建按钮  事项创建
		created() {
			let id = Date.parse(new Date());
			if (!this.value) {
				uni.showToast({
					title: '请输入内容',
					icon: 'none'
				});
				return;
			}
			// 向数组头部添加元素
			this.list.unshift({
				id,
				content: this.value,
				checked: false
			});
			// 添加完毕后，清空输入框，并关闭
			this.value = '';
			this.createShow();
		},
		// 事项完成
		finish(id) {
			console.log(id);
			let index = this.list.findIndex(item => item.id === id);
			this.list[index].checked = !this.list[index].checked;
		},
		// 状态切换
		tab(type) {
			this.activedIndex = type;
		}
	}
};
</script>

<style lang="less">
@import '@/common/icon.css';
.todo-header {
	box-sizing: border-box;
	display: flex;
	align-items: center;
	padding: 0 15px;
	justify-content: space-between;
	font-size: 12px;
	color: #333;
	height: 45px;
	box-shadow: -1px 1px 5px 0 rgba(0, 0, 0, 0.1);
	width: 100%;
	position:fixed;
	top: 0;
	left: 0;
	z-index: 88;
	background-color: #fff;
}
.todo-header text {
	margin-right: 5px;
}
.active-text {
	font-size: 14px;
	color: #279abf;
	padding-right: 10px;
}
.todo-header_right-item {
	padding: 0 5px;
}
.active-tab {
	font-size: 14px;
	color: #279abf;
}

.todo-content {
	padding-top: 50px;
	padding-bottom: 250px;
	position: relative;
}
.todo-list {
	position: relative;
	padding: 20px 15px;
	margin: 15px;
	font-size: 14px;
	box-shadow: -1px 1px 5px 1px rgba(0, 0, 0, 0.1);
	display: flex;
	align-items: center;
	border-radius: 3px;
	overflow: hidden;
}
.todo-list::after {
	position: absolute;
	content: '';
	top: 0;
	left: 0;
	width: 5px;
	bottom: 0;
	background-color: #91d1e8;
}
.checkbox {
	width: 20px;
	height: 20px;
	border-radius: 50%;
	background-color: #eaeaea;
	margin-right: 10px;
	position: relative;
}
.todo-list_content {
	position: relative;
	flex: 1;
	transition: all 0.2s ease-in-out;
}
.todo--finish::after {
	background-color: #ccc;
}
.todo--finish::after {
	background-color: #ccc;
}
.todo--finish .checkbox::after {
	position: absolute;
	content: '';
	border-radius: 50%;
	width: 8px;
	height: 8px;
	background-color: #ccc;
	border: 1px solid #999;
	top: 50%;
	left: 50%;
	transform: translate(-48%, -51%);
}
.todo--finish .todo-list_content {
	color: #b1b1b1;
	padding-left: 10px;
}
.todo--finish .todo-list_content::after {
	position: absolute;
	content: '';
	top: 50%;
	left: 0;
	width: 100%;
	height: 2px;
	background-color: #a2a2a2;
}
.create-todo {
	position: fixed;
	bottom: 20px;
	left: 0;
	right: 0;
	margin: 0 auto;
	width: 60px;
	height: 60px;
	text-align: center;
	font-size: 40px;
	line-height: 60px;
	color: #c1c1c1;
	transition: transform 0.3s ease-in-out;
}
.rotate {
	transform: rotate(135deg);
	transform-origin: center center;
}
.create-content {
	position: fixed;
	bottom: 95px;
	left: 20px;
	right: 20px;
	transition: all .3s ease-in-out;
	opacity: 0;
	transform: scale(0) translateY(200%);
}

.create-show{
	opacity: 1;
	transform: scale(1) translateY(0);
}

.create-content-box {
	background-color: #fff;
	padding: 15px;
	border-radius: 10px;
	box-shadow: -1px 1px 5px 1px rgba(0, 0, 0, 0.1);
	display: flex;
	align-items: center;
	justify-content: space-between;
	z-index: 9;
}
.create-input {
	flex: 1;
}
.create-input textarea {
	width: 100%;
	height: 100px;
	color: #91d1e8;
}
.create-content::after {
	content: '';
	position: absolute;
	left: 0;
	right: 0;
	bottom: -8px;
	margin: 0 auto;
	width: 20px;
	height: 20px;
	background-color: #fff;
	transform: rotate(45deg);
	box-shadow: -1px 1px 5px 1px rgba(0, 0, 0, 0.1);
	z-index: -1;
}
.create-content::before {
	content: '';
	position: absolute;
	left: 0;
	right: 0;
	bottom: 0px;
	margin: 0 auto;
	width: 50px;
	height: 20px;
	background-color: #fff;
}
.create-content-box::after {
	content: '';
	position: absolute;
	left: 0;
	right: 0;
	bottom: -8px;
	margin: 0 auto;
	width: 20px;
	height: 20px;
	background-color: #fff;
	transform: rotate(45deg);
}

.create-button {
	flex: 0 0 40px;
	color: #ccc;
	margin-left: 10px;
	transition: color 0.2s ease-in-out;
}
.btn-color {
	color: #666;
}

// 没有数据显示项
.noList {
	width: 100vw;
	color: #cacaca;
	text-align: center;
	padding-top: 100rpx;
}
.noList .iconfont {
	font-size: 300rpx;
}
</style>
