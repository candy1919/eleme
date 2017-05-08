<template>
	<div v-show="show" class="food">
		<div class="image-header">
			<img :src="selectedFood.image">
			<i class="icon-arrow_lift"></i>
		</div>
		<div class="food-detail">
			<h3 class="name">{{selectedFood.name}}</h3>
			<div class="sell">
				<span>月售{{selectedFood.sellCount}}份</span><span>好评率{{selectedFood.rating}}</span>
			</div>
			<div class="price">
				<span class="newprice">{{selectedFood.price}}</span>
				<span class="oldprice">{{selectedFood.oldPrice}}</span>
			</div>
			<div class="cart">
				<span class="add" v-if="!selectedFood.count">加入购物车</span>
				<cartControl v-else></cartControl>
			</div>
		</div>
		<split></split>
		<div class="content">
			<h3 class="title">商品介绍</h3>
			<p class="text">{{selectedFood.info}}</p>
		</div>
		<div class="content">
			<h3 class="title">商品评价</h3>
			<ratingselect></ratingselect>
			<div class="list">
				<div class="list-item">
					<div class="user">
						<div class="time">
							<span></span><span></span>
						</div>
						<div class="user">
							<span></span><img>
						</div>
					</div>
					<div class="rating">
						<i></i>
						<p class="rating-text"></p>
					</div>
				</div>	
			</div>
		</div>
	</div>
	
</template>
<script>
	import ratingselect from 'components/ratingselect/ratingselect'
	import split from 'components/split/split'
	export default{
		props:{
			selectedFood:{
				type:Object
			},
			show:{
				type:Boolean
			}
		},
		components:{
			ratingselect,
			split
		}
	}
</script>
<style lang="less" scoped>
	.food{
		position:fixed;
		top:0;
		left: 0;
		right: 0;
		bottom: 60px;
		z-index: 20;
		background-color: #fff;
		.image-header{
			position: relative;
			width: 100%;
			padding-top: 100%;
			img{
				position: absolute;
				left: 0;
				top: 0;
				width: 100%;
				height: 100%;
			}
			.icon-arrow_lift{
				position: absolute;
				left:20px;
				top:20px;
				color: #fff;
				font-size: 10px;
			}

		}
		.food-detail{
			position: relative;
			padding: 18px;
			.name{
				line-height: 14px;
				font-size: 14px;
				font-weight: 700;
				color: rgb(7, 17, 27);
			}
			.sell{
				margin: 8px 0 18px 0;
				span{
					line-height: 10px;
					font-size: 10px;
					color: rgb(147, 153, 159);
					&:first-child{
						margin-right: 12px;
					}
				}
			}
			.price{
				font-size: 0;
				.newprice{
					margin-right: 7px;
					line-height: 14px;
					font-size: 14px;
					font-weight: 700;
					color: rgb(240, 20, 20);
				}
				.oldprice{
					line-height: 14px;
					font-size: 10px;
					color: rgb(147, 153, 159);
					text-decoration: line-through;
				}
			}
			.cart{
				position: absolute;
				right: 18px;
				bottom: 18px;
				.add{
					padding: 6px 12px;
					line-height: 12px;
					font-size: 10px;
					color: #fff;
					background-color: rgb(0, 160, 220);
					border-radius: 12px;
				}
			}
		}
	}
</style>