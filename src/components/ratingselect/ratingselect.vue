<template>
	<div class="ratingselect">
		<div class="select-tags">
			<div class="tag all">
				<span>全部</span><span class="num">55</span>
			</div>
			<div class="tag positive">
				<span>满意</span><span class="num">55</span>
			</div>
			<div class="tag negative">
				<span>不满意</span><span class="num">55</span>
			</div>
		</div>
		<div class="onlycontent">
			<i class="icon-check_circle"></i>
			<span class="content">只看有内容的评价</span>
		</div>
	</div>
</template>
<script>
	export default{
		props:{
			ratingtype:{
				type:Number,
				default:2
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
		}
	}
</script>
<style lang="less" scoped>
	.ratingselect{
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
			&.all{
				color: #fff;
				background-color: rgb(0, 160, 220);
			}
		}
	}
</style>