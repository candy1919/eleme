<template>
	<div class="bg-header">
		<div class="bg1"></div>
		<div class="bg2">
		<div class="inf-seller">
			<div class="wrap-avatar"><img src="../../assets/upload/seller/header-bg.jpg"></div>
			<div class="content-area">
				<div class="wrap-name">
					<i class="i-brand"></i>
					<span class="name">{{seller.name}}</span>
				</div>
				<div class="inf-detach">
					<span class="detach">{{seller.description}}</span>/
					<span class="time">{{seller.deliveryTime}}分钟送达</span>
				</div>
				<div class="wrap-discount">
					<div class="pay" v-if="seller.supports">
						<i :class="iconMap[seller.supports[0].type]"></i>
						<p class="content">{{seller.supports[0].description}}</p>
					</div>
					<a class="num-discount" v-on:click.stop.prevent="showDetail" v-if="seller.supports"><span class="num">{{seller.supports.length}}</span>个<i class="icon-keyboard_arrow_right"></i></a>
				</div>
			</div>
		</div>
		<div class="wrap-bulletin">
			<i class="i-bulletin"></i>
			<p class="content">{{seller.bulletin}}</p>
			<i class="icon-keyboard_arrow_right"></i>
		</div>
		</div>
		<transition name="fade">
		    <div class="detail" v-if="isShow" >
				<div class="detail-wrap clearfix">
					<div class="content">
						<p class="name">{{seller.name}}</p>
						<star :size="48" :score="seller.score" class="star"></star>
						<div class="dis-inf">
							<div class="verline"></div>
							<div class="inf">优惠信息</div>
							<div class="verline"></div>
						</div>
						<div class="discount" v-for="support in seller.supports">
							<i :class="iconMap[support.type]" class="icon-item"></i>
							<p>{{support.description}}</p>
						</div>
						<div class="dis-inf">
							<div class="verline"></div>
							<div class="inf">商家公告</div>
							<div class="verline"></div>
						</div>
						<div class="discount">
							<p>{{seller.bulletin}}</p>
						</div>
					</div>
				</div>
			<div class="close">
				<i @click="close" class="icon-close"></i>
			</div>
			</div>
		</transition>
	</div>		
</template>
<script>
import star from 'components/star/star'
	export default{
		props:['seller'],
		components:{
			star
		},
		data(){
			return{
				iconMap:['i-decrease','i-discount','i-special','i-invoice','i-guarantee'],
				isShow:false,
			}
		},
		methods:{
			showDetail(){
				this.isShow=true;
				
			},
			close(){
				this.isShow=false;
				console.log(this.isShow)
			}
		}
	}
</script>
<style lang="less" scoped>
@import "../../common/less/config.less";
.i{
	vertical-align: bottom;
	display: inline-block;
	background-size: 100%;
}
.fade-enter-active,.fade-leave {
  transition: all .8s ease;
  opacity: 1;
  // background: rgba(7, 17,27,0.5);
}
.fade-enter, .fade-leave-active {
	transition: all .8s ease;
  opacity: 0;
}
.bg-header{
	position: relative;
	.bg1{
		position:absolute;
		top:0;
		left:0;
		bottom:10px/@devicePixelRatio;
		right:0;
		background:url("../../assets/upload/seller/header-bg.jpg");
   		filter: blur(10px/@devicePixelRatio);
	}
	.bg2{
		position:relative;
		background: rgba(7, 17,27,0.5);
	}
	.inf-seller{
		display: flex;
		@w1:48px/@devicePixelRatio;
		@w2:24px/@devicePixelRatio;
		@w3:36px/@devicePixelRatio;
		padding: @w1 @w1 @w2 @w3
	}
	.detail{
		position: fixed;
		box-sizing: border-box;
		padding: 0 72px/@devicePixelRatio;
		top:0;
		left:0;
		width: 100%;
		height: 100%;
		overflow: auto;
		z-index: 100;
		color: #fff;
		background: rgba(7, 17, 27, 0.8);
		.detail-wrap{
			min-height: 100%;
			width: 100%
		}
		.content{
			margin-top: 64px;
			padding-bottom: 64px;
			.name{
				margin-bottom: 32px/@devicePixelRatio;
				line-height: 32px/@devicePixelRatio;
				font-weight: 700;
				font-size: 32px/@devicePixelRatio;
				text-align: center;
			}
			.star{
				margin: 32px 0 56px 0;
				text-align: center;
			}
			.dis-inf{
				display: flex;
				margin-bottom: 48px/@devicePixelRatio;
			}
			.inf{
				margin: 0 24px/@devicePixelRatio;
				font-size: 14px;
				font-weight: 700;
			}
			.verline{
				flex:1;
				position: relative;
				top:-6px;
				border-bottom: 1px solid rgba(255, 255, 255, 0.2);
			}
			.discount{
				display: flex;
				margin: 0 0 24px/@devicePixelRatio 24px/@devicePixelRatio;
				p{
					display: inline-block;
					flex:1;
					font-size: 24px/@devicePixelRatio;
					font-weight: 200;
					line-height: 48px/@devicePixelRatio;
				}
				.icon-item{
					width: 32px/@devicePixelRatio;
					height: 32px/@devicePixelRatio;
				}
				&:last-child{
					margin-bottom: 56px/@devicePixelRatio;
				}
			}
		}
		.close{
			position: relative;
			margin: -64px auto 0 auto;
			width: 32px;
			height: 32px;
			clear: both;
			font-size: 32px;
		}
	}
}
.inf-seller{
	.wrap-avatar{
		display: inline-block;
		margin-right: 32px/@devicePixelRatio;
	} img{
		@siz:128px/@devicePixelRatio;
		width:@siz;
		height:@siz;
		border-radius: 4px/@devicePixelRatio;
	}
	.content-area{
		flex:1;
		padding: 4px/@devicePixelRatio 0;
	}
}
.wrap-name{
	.i-brand{
		.i;
		width: 60px/@devicePixelRatio;
		height: 36px/@devicePixelRatio;
		margin-right: 12px/@devicePixelRatio;
		background-image: url('../../assets/header/brand@2x.png');
	}
	.name{
		display: inline-block;
		font-size: 32px/@devicePixelRatio;
		color: rgb(255,255,255);
		font-weight: bold;
		line-height: 36px/@devicePixelRatio;
	}
}
.inf-detach{
	margin: 16px/@devicePixelRatio 0 6px/@devicePixelRatio 0;
	font-size: 24px/@devicePixelRatio;
	color: rgb(255, 255, 255);
	font-weight: 200px/@devicePixelRatio;
	line-height: 24px/@devicePixelRatio;
}
.wrap-discount{
	overflow: hidden;
	font-size: 20px/@devicePixelRatio;
	color: rgb(255, 255, 255);
	font-weight: 200/@devicePixelRatio;
	line-height: 24px/@devicePixelRatio;
	.pay{
		display: inline-block;
		padding: 14px/@devicePixelRatio 0; 
	}

	.content{
		display: inline-block;
	}
	.num-discount{
		@w1:14px/@devicePixelRatio;
		@w2:16px/@devicePixelRatio;
		float:right;
		padding: @w1 @w2;
		border-radius: @w2;
		background-color:rgba(0, 0, 0, 0.2);
		i{
			margin-left: 4px/@devicePixelRatio;
		}
	}
}
.wrap-bulletin{
	display: flex;
	align-items:center;
	box-sizing: border-box;
	padding:  0 24px/@devicePixelRatio;
	height: 58px/@devicePixelRatio;
	font-size: 20px/@devicePixelRatio;
	color: rgb(255, 255, 255);
	background-color: rgba(7,17, 27, 0.2);

	.i-bulletin{
		.i;
		width: 44px/@devicePixelRatio;
		height: 24px/@devicePixelRatio;
		margin-right: 8px/@devicePixelRatio;
		background-image: url('../../assets/header/bulletin@2x.png');
	}
	.content{
		flex:1;
		margin-right: 8px/@devicePixelRatio;
		overflow: hidden;
		white-space: nowrap;
		text-overflow:ellipsis;
	}
}
		.i-decrease{
		.i;
		@siz:24px/@devicePixelRatio;
		width: @siz;
		height: @siz;
		margin-right: 8px/@devicePixelRatio;
		background-image: url('../../assets/header/decrease_1@2x.png');
	}
	.i-discount{
		.i;
		@siz:24px/@devicePixelRatio;
		width: @siz;
		height: @siz;
		margin-right: 8px/@devicePixelRatio;
		background-image: url('../../assets/header/discount_1@2x.png');
	}
	.i-special{
		.i;
		@siz:24px/@devicePixelRatio;
		width: @siz;
		height: @siz;
		margin-right: 8px/@devicePixelRatio;
		background-image: url('../../assets/header/special_1@2x.png');
	}
	.i-invoice{
		.i;
		@siz:24px/@devicePixelRatio;
		width: @siz;
		height: @siz;
		margin-right: 8px/@devicePixelRatio;
		background-image: url('../../assets/header/invoice_1@2x.png');
	}
	.i-guarantee{
		.i;
		@siz:24px/@devicePixelRatio;
		width: @siz;
		height: @siz;
		margin-right: 8px/@devicePixelRatio;
		background-image: url('../../assets/header/guarantee_1@2x.png');
	}
</style>