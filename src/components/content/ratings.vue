<template>
	<div v-show="show">
		<img>
		<div>
			<h3>{{selectedFood.name}}</h3>
			<div>
				<span>月售{{selectedFood.sellCount}}份</span><span>好评率{{selectedFood.rating}}</span>
			</div>
			<div>
				<span>{{selectedFood.price}}</span>
				<span>{{selectedFood.oldPrice}}</span>
				<span v-if="!selectedFood.count">加入购物车</span>
				<cartControl v-else></cartControl>
			</div>
		</div>
		<div>
			<h3>商品介绍</h3>
			<p>{{selectedFood.info}}</p>
		</div>
		<div>
			<h3>商品评价</h3>
			<ratingselect></ratingselect>
			<div class="list">
				<div>
					<img>
					<div>
						<div>
							<span>姓名</span>
							<span>时间</span>
						</div>
						<star></star><span>80分钟送达</span>
						<p>pingjia</p>
						<div>
							<i></i>
							<span>陆凡</span>
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
	
</template>
<script>
	export default{
		props:{
			selectedFood:{
				type:Object
			},
			show:{
				type:Boolean
			}
		}
	}
</script>