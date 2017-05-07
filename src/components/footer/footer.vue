<template>
	<div class="footer">
		<div class="inf-cart">
			<div class="wrap-cart">
				<i class="icon-shopping_cart" :class="{hightlight:totalCount}" @click="showList"></i>
				<span class="count" v-if="totalCount">{{totalCount}}</span>
			</div>
			<p class="total" :class="{hightlight:totalPrice}">￥{{totalPrice}}</p>
			<p class="line-vertical wrap"></p>
			<p class="send">配送费￥{{deliveryPrice}}元</p>
		</div>
		<div class="submit"  :class="{enough:totalPrice>=this.minPrice}">
			<a>{{payDesc}}</a>
		</div>
		<div class="ball-container">
			<div v-for="ball in balls">
				<transition name="drop" @before-enter="beforeDrop" @enter="dropping" @after-enter="afterDrop">
					<div class="ball" v-show="ball.show">
						<div class="inner inner-hook"></div>
					</div>
				</transition>
			</div>
		</div>
		<transition name="fade">
		<div class="shopcart-list" v-if="cartShow">
			<div class="header">
				<span class="title">购物车</span>
				<span class="empty">清空</span>
			</div>
			<div class="content-list">
				<ul>
					<li v-for="item in selectFood" class="item border-1px">
						<span class="name">{{item.name}}</span>
						<div class="control">
							<span class="price">￥{{item.price}}元</span>
							<cartControl :food="item" @add="dropball"></cartControl>
						</div>
					</li>
				</ul>
			</div>
		</div>
		</transition>
	</div>
</template>
<script>
import cartControl from 'components/cartControl/cartControl'
	export default{
		components:{
			cartControl,
		},
		props:{
			deliveryPrice:{
				type:Number,
				default:0
			},
			minPrice:{
				type:Number,
				default:0
			},
			selectFood:{
				type:Array,
				default(){
					return 	[]		
				}
			}
		},
		data(){
			return {
				balls:[
					{show:false},
					{show:false},
					{show:false},
					{show:false},
					{show:false},
					{show:false},
					{show:false},
				],
				dropballs:[],
				cartShow:false,
			}
		},
		computed:{
			totalPrice(){
				let total=0;
				this.selectFood.forEach(food=>{
					total+=food.price*food.count;
				})
				return total
			},
			totalCount(){
				let count=0;
				this.selectFood.forEach(food=>{
					count+=food.count;
				})
				return count
			},
			payDesc(){
				let total=0;
				this.selectFood.forEach(food=>{
					total+=food.price*food.count;
				});
				if(total<this.minPrice&&total>0){
					return `还差${this.minPrice-total}元`
				}else if(total===0){
					return `${this.minPrice}元起送`
				}else{
					return '去结算'
				}
			}
		},
		methods:{
			showList(){
				this.cartShow=true;
			},
			dropball(el){
				for(let i=0;i<this.balls.length;i++){
					let ball=this.balls[i];
					if(!ball.show){
						ball.show=true;
						ball.el=el;
						ball.index=i;
						this.dropballs.push(ball);
						return ;
					}
				}
			},
			beforeDrop(el){
				//把使用到的小球从起始位置（购物车位置）上升到添加按钮位置
				let count=this.balls.length;
				while(count--){
					let ball=this.balls[count];
					if(ball.show){
						let rect=ball.el.getBoundingClientRect();
						let y=-(window.innerHeight-rect.top-25)
						let x=rect.left-32;
						let inner=el.getElementsByClassName("inner-hook")[0];
						el.style.display="";
						el.style.webkitTransform=`translate3d(0,${y}px,0)`;
						el.style.transform=`translate3d(0,${y}px,0)`;
						inner.style.webkitTransform=`translate3d(${x}px,0,0)`;
						inner.style.transform=`translate3d(${x}px,0,0)`;
						
					}
				}
			},
			dropping(el,done){
				let height=el.offsetHeight//触发重绘
				this.$nextTick(()=>{
					let inner=el.getElementsByClassName("inner-hook")[0];
					el.style.webkitTransform='translate3d(0,0,0)';
					el.style.transform='translate3d(0,0,0)';
					inner.style.webkitTransform='translate3d(0,0,0)';
					inner.style.transform='translate3d(0,0,0)';
					el.addEventListener('transitionend', done);
					 //Vue为了知道过渡的完成，必须设置相应的事件监听器。

				})
			},
			afterDrop(el){
				let ball=this.dropballs.shift();
				if(ball){
					ball.show=false;
					el.style.display="none"
				}
				
			},

		}
	}
</script>
<style lang="less" scoped>
	@import "../../common/less/config.less";
	@import "../../common/less/common.less";
	.wrap{
		margin: 0 24px/@devicePixelRatio;
	}
	.footer{
		display: flex;
		left: 0;
		right: 0;
		bottom: 0;
		position:fixed;
		padding-left:120px/@devicePixelRatio; 
		background: #141d27;
		.inf-cart{
			flex:1;
		}
		.wrap-cart{
			position: absolute;
			left:18px/@devicePixelRatio;
			top:-10px;
			margin-right: 18px/@devicePixelRatio;
			padding: 12px/@devicePixelRatio;
			border-radius: 50%;
			background: #141d27;
			i{
				display: flex;
			    align-items: center;
			    justify-content: center;
				width: 88px/@devicePixelRatio;
				height: 88px/@devicePixelRatio;
				background: rgba(0, 0, 0, 0.2);
				font-size: 48px/@devicePixelRatio;
				color: rgba(255, 255, 255, 0.4);
				line-height: 48px/@devicePixelRatio;
				border-radius: 50%;
				&.hightlight{
					color:#fff;
					background-color: rgb(0, 160, 220);
				}
			}
			.count{
				display: inline-block;
				position: absolute;
				top:0;
				right: 0;
				padding: 0 6px;
				line-height: 16px;
				font-size: 9px;
				font-weight: 700;
				color:rgb(255, 255, 255);
				background: red;
				border-radius: 16px;
			}
		}
		p{
			display: inline-block;
			padding: 24px/@devicePixelRatio 0;
		}
		.total{
			font-size: 32px/@devicePixelRatio;
			color: rgba(255, 255, 255, 0.4);
			font-weight: 700;
			line-height: 48px/@devicePixelRatio;
			&.hightlight{
				color: #fff
			}
		}
		.send{
			font-size: 20px/@devicePixelRatio;
			color: rgba(255, 255, 255, 0.4);
			line-height: 48px/@devicePixelRatio;
		}
		.submit{
			display: flex;
		    align-items: center;
		    justify-content: center;
			box-sizing: border-box;
			width:210px/@devicePixelRatio;
			padding: 0 16px/@devicePixelRatio;
			background: rgba(0, 0, 0, 0.2);
			a{
				font-size: 24px/@devicePixelRatio;
				color: rgba(255, 255, 255, 0.4);
				font-weight: 700/@devicePixelRatio;
				line-height: 48px/@devicePixelRatio;
			}
			&.enough{
				font-weight: 700;
				color: #fff;
				background: #2b9422;
			}
		}
		.ball-container{
			.ball{
				position: fixed;
				z-index: 50;
				left:32px;
				bottom:25px;
				transition:0.6s all cubic-bezier(0.49, -0.29, 0.75, 0.41)
			}
			.inner{
				width: 12px;
				height: 12px;
				background: rgb(0, 160, 220);
				border-radius: 50%;
				transition:0.6s all linear;
			}
		}
		.shopcart-list{
			position: absolute;
			bottom: 0;
			left: 0;
			width: 100%;
			z-index:-1;
			padding-bottom:50px;
			.header,.content-list{
				padding: 0 36px/@devicePixelRatio;
			}
			.header{
				overflow: hidden;
				border-bottom: 2px solid rgba(7, 17, 27, 0.1);
				background-color: #f3f5f7;
				.title{
					float: left;
					line-height: 80px/@devicePixelRatio;
					font-size: 28px/@devicePixelRatio;
					font-weight: 200;
					color:rgb(7, 17, 27);
				}
				.empty{
					float: right;
					line-height: 80px/@devicePixelRatio;
					font-size: 24px/@devicePixelRatio;;
					color:rgb(0, 160, 220);
				}
			}
			.content-list{
				background-color: #fff;
				.item{
					position: relative;
					.border(rgb(7, 17, 27));
					.control{
						position: absolute;
						right:0;
						bottom: 15px;
						padding-left: 36px;
					}
					.name{
						display: inline-block;
						padding: 24px/@devicePixelRatio 0;
						line-height: 48px/@devicePixelRatio;
						font-size: 28px/@devicePixelRatio;;
						color:rgb(7, 17, 27);
					}
					.price{
						margin-right: 24px/@devicePixelRatio;
						line-height: 48px/@devicePixelRatio;
						font-size: 28px/@devicePixelRatio;
						font-weight: 700;
						color:rgb(240, 20, 20);
					}
				}
			}
		}
		.fade-enter-active{
		  transition: all .3s linear;
		}
		.fade-leave{
			transition: all .3s linear;
		}
		.fade-enter, .fade-leave-active {
		  transform: translate3d(0,100%,0);
		  opacity: 0;
		}		
	}
</style>