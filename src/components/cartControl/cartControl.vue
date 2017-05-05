<template>
	<div class="cart">
		<transition name="fade">
			<div v-if="food.count">
				<a class="icon-remove_circle_outline" @click="descrease"></a>
				<span class="num">{{food.count}}</span>
			</div>
		</transition>
		<a class="icon-add_circle" @click="add" ref="addbtn"></a>
	</div>
</template>
<script>
import Vue from 'vue'
export default{
	props:{
		food:{
			type:Object
		}
	},
	methods:{
		add(){
			if(!this.food.count){
				Vue.set(this.food,'count',1)
			}else{
				this.food.count++;
			}
			let el=this.$refs.addbtn;
			this.$emit('add',el);
		},
		descrease(){
			this.food.count--;
		}
	}
}
</script>
<style lang="less" scoped>
@import "../../common/less/config.less";
.fade-enter-active {
  transition: all .3s ease;
}
.fade-leave{
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.fade-enter, .fade-leave-active {
  transform: translateX(10px);
  opacity: 0;
}
.cart{
	float: right;
	display: flex;
	a{
		font-size: 48px/@devicePixelRatio;
		color:rgb(0,160,220);
		line-height: 48px/@devicePixelRatio;
	}
	.num{
		display: inline-block;
		margin: 0 48px/@devicePixelRatio;
		line-height: 48px/@devicePixelRatio;
		vertical-align: top;
		font-size: 10px;
		color: rgb(147,153,159);
	}
}
</style>