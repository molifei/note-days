<template>
	<view class="content">
		<view class="todo-header">
			<!-- 状态栏的左侧 -->
			<view class="tode-header_left">
				<text class="active-text">全部</text>
				<text>10条</text>
			</view>
			<!-- 右侧 -->
			<view class="tode-header_right">
				<text class="todo-header_right-item active-tab">全部</text>
				<text class="todo-header_right-item">待办</text>
				<text class="todo-header_right-item">已完成</text>
			</view>
		</view>

		<view class="todo-content">
			<view class="todo-list todo--finish">
				<view class="todo-list_checkbox"><view class="checkbox"></view></view>
				<view class="todo-list_content">我的待办事项</view>
			</view>
			<view class="todo-list">
				<view class="todo-list_checkbox"><view class="checkbox"></view></view>
				<view class="todo-list_content">我的待办事项</view>
			</view>
		</view>

		<!-- 创建按钮 -->
		<text class="create-todo iconfont icon-add"></text>
		<!-- 创建的元素 -->
		<view class="create-content">
			<div class="create-content-box">
				<view class="create-input"><textarea type="text" value="" placeholder="输入事项" v-model="value" /></view>
				<view class="create-button">创建</view>
			</div>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			title: 'Hello',
			value:""
		};
	},
	onLoad() {console.log(this.value)},
	methods: {
		
	}
};
</script>

<style lang="less">
@import '@/common/icon.css';
.todo-header {
	display: flex;
	align-items: center;
	padding: 0 15px;
	justify-content: space-between;
	font-size: 12px;
	color: #333;
	height: 45px;
	box-shadow: -1px 1px 5px 0 rgba(0, 0, 0, 0.1);
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
	left: 50%;
	transform: translateX(-50%);
	width: 60px;
	height: 60px;
	text-align: center;
	font-size: 40px;
	color: #c1c1c1;
}
.create-content {
	position: fixed;
	bottom: 95px;
	left: 20px;
	right: 20px;
}
.create-content-box{
	position: fixed;
	bottom: 95px;
	left: 20px;
	right: 20px;
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
}
</style>
