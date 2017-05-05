<template>
	<div class="footer">
		<div class="inf-cart">
			<div class="wrap-cart">
				<i class="icon-shopping_cart" :class="{hightlight:totalCount}"></i>
				<span class="count" v-if="totalCount">{{totalCount}}</span>
			</div>
			<p class="total" :class="{hightlight:totalPrice}">￥{{totalPrice}}</p>
			<p class="line-vertical wrap"></p>
			<p class="send">另需配送费￥{{deliveryPrice}}元</p>
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
		
	</div>
</template>
<script>
	export default{
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
				dropballs:[]
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
			dropball(el){
				for(let i=0;i<this.balls.length;i++){
					let ball=this.balls[i];
					if(!ball.show){
						ball.show=true;
						ball.el=el;
						this.dropballs.push(ball);
						return ;
					}
				}
			},
			beforeDrop(el){
				let count=this.balls.length;
				while(count--){
					let ball=this.balls[count];
					if(ball.show){
						let rect=ball.el.getBoundingClientRect();
						console.log(rect)
						let x=rect.left-30;
						let y=-(window.innerHeight-rect.top-25)
						el.style.display='';
						el.style.webkitTransform=`translate3d(0,${y}px,0)`;
						el.style.transform=`tranlate3d(0,${y}px,0)`;
						let inner=el.getElementsByClassName('inner-hook')[0];
						inner.style.webkitTransform=`translate3d(${x}px,0,0)`;
						inner.style.transform=`tranlate3d(${x}px,0,0)`;
					}
				}
			},
			dropping(el,done){
				let rf=el.offsetHeight;
				this.$nextTick(()=>{
					el.style.webkitTransform='tranlate3d(0,0,0)';
					el.style.transform='translate3d(0,0,0)';
					let inner=el.getElementsByClassName('inner-hook')[0];
					inner.style.webkitTransform='translate3d(0,0,0)';
					inner.style.transform='translate3d(0,0,0)';
					el.addEventListener('transitionend', done);
				})
			},
			afterDrop(el){
				console.log(222)
				let ball=this.dropballs.shift();
				if(ball){
					ball.show=false;
					el.style.display='none'
				}
			},
		}
	}
</script>
<style lang="less" scoped>
	@import "../../common/less/config.less";
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
			font-size: 32px/@devicePixelRatio;
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
				left:32px;
				bottom: 22px;
				z-index: 200;
				transition:all 0.6s cubic-bezier(0.49,-0.29,0.75,0.41)
				
			}
			.inner{
				width: 15px;
				height: 15px;
				background: rgb(0, 160, 220);
				border-radius: 50%;
				transition:all 0.6s linear
			}
		}
		
	}
</style>