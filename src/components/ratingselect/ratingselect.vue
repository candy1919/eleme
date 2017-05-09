<template>
	<div class="ratingselect">
		<div class="select-tags border-1px">
			<div class="tag positive" :class="{active:ratingtype===2}" @click.stop="show(2)">
				<span>{{desc.all}}</span><span class="num">{{ratings.length}}</span>
			</div>
			<div class="tag positive" :class="{active:ratingtype===0}" @click.stop="show(0)">
				<span>{{desc.positive}}</span><span class="num">{{positives.length}}</span>
			</div>
			<div class="tag negative" :class="{active:ratingtype===1}" @click.stop="show(1)">
				<span>{{desc.negative}}</span><span class="num">{{negatives.length}}</span>
			</div>
		</div>
		<div class="onlycontent">
			<i class="icon-check_circle" :class="{select:onlycontent}" @click.stop="toggleContent"></i>
			<span class="content">只看有内容的评价</span>
		</div>
	</div>
</template>
<script>
	const POSITIVE=0;
	const NEGATIVE=1;
	const ALL=2;
	export default{
		props:{
			ratings:{
				type:Array,
				default(){
					return []
				}
			},
			ratingtype:{
				type:Number,
				default:ALL
			},
			onlycontent:{
				type:Boolean,
				default:false
			},
			desc:{
				type:Object,
				default(){
					return {
						all:'全部',
						positive:'满意',
						negative:'吐槽'
					}
				}
			}
		},
		computed:{
			positives(){
				return this.ratings.filter((rating)=>{
					return rating.rateType==0
				})
			},
			negatives(){
				return this.ratings.filter((rating)=>{
					return rating.rateType==1
				})
			}
		},
		methods:{
			show(type){
				this.$emit('changetype',type);
			},
			toggleContent(){
				this.$emit('changecontent');
			}
		}
	}
</script>
<style lang="less" scoped>
	@import "../../common/less/common.less";
	.ratingselect{
		.select-tags{
			padding-bottom: 18px;
			.border(rgba(7, 17, 27, 0.1)) ;
		}
		.tag{
			display: inline-block;
			margin-right: 8px;
			padding: 8px 12px;
			line-height: 16px;
			font-size: 12px;
			.num{
				margin-left: 5px;
				font-size: 8px;
			}
		}
		.positive{
			color: rgb(77, 85, 93);
			background-color: rgba(0, 160, 220,0.2);
			&.active{
				color:#fff;
				background-color: rgb(0, 160, 220);
			}
		}
		.negative{
			color: rgb(77, 85, 93);
			background-color: rgba(77, 85, 93, 0.2);
			&.active{
				color:#fff;
				background-color: rgb(77, 85, 93);
			}
		}
		.onlycontent{
			padding: 12px 0;
			font-size: 0;
			.icon-check_circle{
				display: inline-block;
				margin-right: 4px;
				line-height: 24px;
				vertical-align: middle;
				font-size: 24px;
				color:rgb(147, 153, 159);
				&.select{
					color:#00a0dc;
				}
			}
			.content{
				display: inline-block;
				line-height: 24px;
				vertical-align: middle;
				font-size: 12px;
				color:rgb(147, 153, 159);
			}
		}
	}
</style>