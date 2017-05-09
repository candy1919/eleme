<template>
	<div class="wrap-goods">
		<div class="nav" ref="menu">
			<div class="nav-item" v-for="(item,index) in goods" :class="{'current':currentIndex==index}" @click="selectMenu(index)">
				<i class="i-special"></i>
				<span>{{item.name}}</span>
			</div>
		</div>
		<div class="list" ref="list" @scroll="calculateScrollY">
		<div v-for="item in goods" class="food-hook">
			<p class="title">{{item.name}}</p>
			<div class="wrap-item" v-for="food in item.foods"  @click="chosefood(food)">
			<div class="list-item">
				<div class="wrap-avatar">
					<img :src="food.icon">
				</div>
				<div class="item-detail">
					<p class="name">{{food.name}}</p>
					<p class="type">{{food.description}}</p>
					<p class="description"><span>月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span></p>
					<div class="price">
						<p class="price-now">￥{{food.price}}</p>
						<p class="price-once" v-if="food.oldPrice">￥{{food.oldPrice}}</p>
						<cartControl :food="food"  @add="addcart"></cartControl>
					</div>
				</div>
			</div>
			</div>
		</div>
		</div>
		<food :show="foodShow" :selectedFood="fooditem" @quit="quit" @add="addcart"></food>
		<vFooter :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice" :selectFood="selectFood" ref="cart"></vFooter>
	</div>
</template>
<script>
import cartControl from 'components/cartControl/cartControl'
import food from 'components/food/food'
import Vue from 'vue'
export default{
	components:{
		cartControl,
		food
	},
	props:{
		seller:{
			type:Object
		}
	},
	data(){
		return{
			goods:[],
			listHeight:[],
			scrollY:0,
			fooditem:{},
			foodShow:false,
		}
	},
	computed:{
		selectFood(){
			let foodlist=[];
			this.goods.forEach(item=>{item.foods.forEach(food=>{if(food.count){foodlist.push(food)}})})
			return foodlist;
		},
		currentIndex(){
			for(let i=0;i<this.listHeight.length;i++){
				let height1=this.listHeight[i];
				let height2=this.listHeight[i+1];
				if(this.scrollY>=height1&&this.scrollY<height2||!height2){
					return i;
				}
			}
			return 0;
		}
	},
	created(){
		this.$http.get('/api/goods').then(response => {
	    this.someData = response.body;
	    if(this.someData.errno==0){
	    	this.goods=this.someData.data;
	    	this.$nextTick(function(){
	    		this.calculateHeight();
	    	})
	    }
	  	}, response => {
	  	});
	},
	methods:{
		quit(){
			this.foodShow=false;	
		},
		chosefood(item){
			this.fooditem=item;
			this.foodShow=true;
		},
		addcart(el){
			let cart=this.$refs.cart;
			cart.dropball(el)
		},
		selectMenu(index){
			let list=this.$refs.list;
			let height=this.listHeight[index];
			if(index>0){
				list.scrollTop=this.listHeight[index];
			}else{
				list.scrollTop=0;
			}
		},
		calculateScrollY(){
			let list=this.$refs.list;
			this.scrollY=list.scrollTop;
		},
		calculateHeight(){
			let foodList=this.$refs.list.getElementsByClassName("food-hook");
			let height=0;
			this.listHeight.push(height);
			for(let i=0;i<foodList.length;i++){
				let item=foodList[i];
				height+=item.clientHeight;
				this.listHeight.push(height)
			}
		}
	}
}
</script>
<style lang="less" scoped>
	@import "../../common/less/config.less";
		::-webkit-scrollbar{
			display: none;
		}
	.font-grey(@siz:20px){
		font-size: @siz/@devicePixelRatio;
		color: rgb(147,153,159);
	}
	.vertical-align(@h:30px){
		top:50%;
		margin-top:-@h/2;
	}
	.wrap-goods{
		display: flex;
		position: fixed;
		top:180px;
		left:0;
		bottom: 58px;
		width: 100%;
	}
	.nav{
		box-sizing: border-box;
		padding:0 24px/@devicePixelRatio;
		width: 160px/@devicePixelRatio;
		height: 100%;
		overflow: auto;
		background:#f3f5f7;
		.nav-item{
			display: flex;
			align-items:center;
			@h:108px/@devicePixelRatio;
			height:@h;
			font-size: 24px/@devicePixelRatio;
			color: #000;
			font-weight: 200/@devicePixelRatio;
			span{
				line-height:28px/@devicePixelRatio;
			}
		}
		.current{
			background: #fff;
		}
	}
	.list{
		flex:1;
		height: 100%;
		overflow: auto;
		.title{
			@h:52px/@devicePixelRatio;
			.font-grey(24px);
			box-sizing: border-box;
			padding-left: 24px/@devicePixelRatio;
			height: @h;
			line-height: @h;
			border-left: 4px/@devicePixelRatio solid #d6dde1;
			background: #f3f5f7;
		}
		.wrap-item{
			margin: 0 36px/@devicePixelRatio;
			border-bottom: 1px solid rgba(7, 17, 27, 0.4);
			&:last-child{
				border:none;
			}
		}
		.list-item{
			display: flex;
			margin: 36px/@devicePixelRatio 0;
		}
	}
	.item-detail{
		flex:1;
		.name{
			padding-top:4px/@devicePixelRatio;
			font-size: 28px/@devicePixelRatio;;
			color: rgb(7, 17, 27);
		}
		.type{
			margin: 16px/@devicePixelRatio 0;
			.font-grey();
		}
		.description{
			margin: 16px/@devicePixelRatio 0;
			.font-grey();
			span{
				margin-right: 24px/@devicePixelRatio;
			}
		}
		.price{
			margin: 16px/@devicePixelRatio 0;
			p{
				display: inline-block;
			}
			.price-now{
				font-size: 28px/@devicePixelRatio;
				font-weight: 700/@devicePixelRatio;
				color: rgb(240, 20, 20);
			}
			.price-once{
				.font-grey();
				text-decoration: line-through;
			}
		}
	}
	.wrap-avatar{
		margin-right: 20px/@devicePixelRatio;
		img{
			@siz:116px/@devicePixelRatio;
			height: @siz;
			width: @siz;
		}
	}
</style>