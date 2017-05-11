<template>
	<div class="seller">
		<div class="wrap">
			<div class="main-inf">
				<h3 class="name">{{seller.name}}</h3>
				<div class="star-wrap">
					<star :size="36" :score="seller.score"></star>
				</div>
				<span class="num">({{seller.ratingCount}})</span>
				<span class="num">月售{{seller.sellCount}}单</span>
				<div class="collect">
					<i class="icon-favorite" @click="collect" :class="{grey:!favorite}"></i>
					<p class="text" v-if="favorite">已收藏</p>
					<p class="text" v-else>收藏</p>
				</div>
			</div>
			<div class="detail">
				<div class="deliver">
					<p class="hit">起送价</p>
					<p class="detail-num"><span class="num">{{seller.minPrice}}</span>元</p>
				</div>
				<div class="deliver">
					<p class="hit">商家配送</p>
					<p class="detail-num"><span class="num">{{seller.deliveryPrice}}</span>元</p>
				</div>
				<div class="deliver">
					<p class="hit">平均配送时间</p>
					<p class="detail-num"><span class="num">{{seller.deliveryTime}}</span>分钟</p>
				</div>
			</div>

		</div>
		<split></split>
		<div class="wrap bullet-warp">
			<h3 class="title">公告与活动</h3>
			<p class="bullet">{{seller.bulletin}}</p>
			<div class="surport">
				<div class="item" v-for="item in seller.supports">
					<i :class="icons[item.type]"></i>
					<p class="text">{{item.description}}</p>
				</div>
			</div>
		</div>
		<split></split>
		<div class="wrap view">
			<h3 class="title">商家实景</h3>
			<div class="list-wrap">
				<div class="image-wrap" v-for="item in seller.pics">
					<img width="120" height="90" :src="item">
				</div>
			</div>
		</div>
		<split></split>
		<div class="wrap contact">
			<h3 class="title">商家信息</h3>
			<div class="item" v-for="item in seller.infos">{{item}}</div>
		</div>
	</div>
</template>
<script>
import star from 'components/star/star'
import split from 'components/split/split'
	export default{
		props:{
			seller:{
				type:Object
			}
		},
		components:{
			star,
			split
		},
		data(){
			return {
				icons:['decrease','discount','special','invoice','guarantee'],
				favorite:false,
			}
		},
		methods:{
			collect(){
				this.favorite=!this.favorite;
			}
		}
	}
</script>
<style lang="less" scoped>
	::-webkit-scrollbar{
		display: none;
	}
	.decrease{
		background-image: url("./decrease_2@2x.png");
	}
	.discount{
		background-image: url("./discount_2@2x.png");
	}
	.guarantee{
		background-image: url("./guarantee_2@2x.png");
	}
	.invoice{
		background-image: url("./invoice_2@2x.png");
	}
	.special{
		background-image: url("./special_2@2x.png");
	}
	.seller{
		padding-bottom: 50px;
		.wrap{
			padding: 18px;
			.title,.name{
				margin-bottom: 8px;
				line-height: 14px;
				font-size: 14px;
				color:rgb(7, 17, 27);
			}
			.star-wrap{
				display: inline-block;
				margin-right: 8px;
			}
			.main-inf{
				position: relative;
				padding-bottom: 18px;
				border-bottom: 1px solid rgba(7,17,27,0.1);
				.num{
					margin-right: 12px;
					line-height: 18px;
					font-size: 10px;
					color: rgb(77, 85, 93);
					&:last-child{
						margin-right: 0;
					}
				}
				.collect{
					position: absolute;
					top:0;
					right: 0;
					text-align: right;
					.icon-favorite{
						margin-bottom: 4px;
						line-height: 24px;
						font-size: 24px;
						color: rgb(240, 20, 20);
						&.grey{
							color:rgba(7, 17, 27, 0.3);
						}
					}
					.text{
						line-height: 10px;
						font-size: 10px;
						color: rgb(77, 85, 93);
					}
				}
			}
			.detail{
				display: flex;
				padding: 18px 0;
				.deliver{
					flex:1;
					text-align: center;
					border-right: 1px solid rgba(7, 17, 27, 0.1);
					.hit{
						margin-bottom: 4px;
						line-height: 10px;
						font-size: 10px;
						color: rgb(147, 153, 159);
					}
					.detail-num{
						line-height: 24px;
						font-size: 10px;
						color:rgb(7, 17, 27);
						.num{
							font-size: 24px;
							font-weight: 200;
						}
					}
					&:last-child{
						border:none;
					}
				}
			}
			.bullet{
				padding: 0 12px 16px 12px;
				line-height: 24px;
				font-size: 12px;
				font-weight: 200;
				color: rgb(240, 20, 20); 
			}
			.surport{
				.item{
					padding: 16px 12px;
					font-size: 0;
					border-top:1px solid rgba(7, 17, 27, 0.1);
				}
				i{
					display: inline-block;
					margin-right: 4px;
					vertical-align: top;
					width: 16px;
					height: 16px;
					background-size: 100% 100%;
				}
				.text{
					display: inline-block;
					line-height: 16px;
					font-size: 12px;
					font-weight: 200;
					color: rgb(7, 17, 27);
				}
			}
			&.bullet-warp{
				padding-bottom: 0;
			}
			&.view,&.contact{
				.title{
					margin-bottom: 12px;
				}
			}
			&.view{
				.list-wrap{
					width: 100%;
					overflow: auto;
					white-space: nowrap;
				}
			}
			.image-wrap{
				display: inline-block;
				margin-right: 6px;
			}
			&.contact{
				.item{
					padding: 16px 12px;
					line-height: 16px;
					font-size: 12px;
					color: rgb(7, 17, 27);
					font-weight: 200;
					border-top: 1px solid rgba(7, 17, 27, 0.1);
				}
			}
		}

	}
</style>