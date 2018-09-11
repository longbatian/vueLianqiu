<template>
	<div class="flex">
		<h2>点击查看运动量</h2>
		<!-- <input type="text" placeholder="请输入你的名字"> -->
		<router-link 
			tag='button'
			to="./Home/1"
		>星星</router-link >	
		<router-link 
			tag='button'
			to="./Home/2"
		>鑫鑫</router-link >	
		<router-link 
			tag='button'
			to="./Home/3"
		>RH</router-link >	
	</div>
</template>
<script>
export default {
	name:'Login',
	
}	
</script>
<style style="less" scoped>
	input{
		display: block;
		margin:0 auto;
		width: 2rem;
		height: .34rem;
}
	button{
		display: block;
		margin:.1rem auto;
		width: 1.5rem;
		/* height: .3rem; */
		color: #fff;
		background: red;
		border: none;
		padding:.08rem .2rem;
	}
	.flex{
		display: flex;
		justify-content: center;
		height: 100%;
		flex-direction: column ;
		text-align: center
		/* text */
	}
</style>