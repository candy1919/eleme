<template>
	<div class="ratingselect">
		<div class="inf">
			<div class="score-wrap border-1px">
				<p class="score">{{totalScore}}</p>
				<p class="title">综合评级</p>
				<p class="total-rating">高于周边</p>
			</div>
			<div class="rating-item">
				<div class="severce">
					<span class="tag">服务态度</span>
					<div class="star-wrap">
						<star  :size="36" :score="totalScore"></star>
					</div>
					<span class="score">{{totalScore}}</span>
				</div>
				<div class="severce">
					<span class="tag">服务态度</span>
					<div class="star-wrap">
						<star :size="36" :score="totalScore"></star>
					</div>
					<span class="score">{{totalScore}}</span>
				</div>
				<div class="severce">
					<span class="tag">送达时间</span>
					<div class="time-wrap">
						<span class="time">{{totalTime}}分钟</span>
					</div>
				</div>
			</div>
		</div>
		<split></split>
		<div class="ratingselect-wrap">
			<ratingselect :ratings="ratings" :ratingtype="ratingtype" :onlycontent="onlycontent" :desc="desc" @changetype="changetype" @changecontent="changecontent"></ratingselect>
		</div>
		<div class="list">
			<div v-if="!listratings.length">暂无评论</div>
			<div v-else class="list-item border-1px" v-for="rating in listratings">
				<div class="image-wrap">
					<img :src="rating.avatar">
				</div>
				<div class="content">
					<p class="name">{{rating.username}}</p>
					<div class="score-wrap">
						<div class="star-wrap">
							<star :size="24" :score="rating.score"></star>
						</div>
						<span class="text">{{rating.deliveryTime}}</span>
					</div>
					<p class="text">{{rating.text}}</p>
					<div class="food-tag">
						<i :class="[rating.rateType===0?'icon-thumb_up':'icon-thumb_down']"></i>
						<span class="tag border-1px" v-for="item in rating.recommend">{{item}}</span>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>
<script>
	import split from 'components/split/split'
	import ratingselect from 'components/ratingselect/ratingselect'
	import star from 'components/star/star' 
	export default{
		components:{
			split,
			ratingselect,
			star
		},
		props:{
			seller:{
				type:Object,
				default(){
					return {}
				}
			}
		},
		data(){
			return {
				ratings:[],
				ratingtype:2,
				onlycontent:false,
				desc:{
					all:'全部',
					positive:'满意',
					negative:'不满意'
				}
			}
		},
		created(){
			this.$http.get('/api/ratings').then(response => {
		    this.someData = response.body;
		    if(this.someData.errno==0){
		    	this.ratings=this.someData.data;
		    }
		  	}, response => {
		  	});
		},
		computed:{
			listratings(){
				let ratings=[];
				if(this.ratingtype!=2){
					ratings=this.ratings.filter((rating)=>{
						return rating.rateType===this.ratingtype
					})
				}else{
					ratings=this.ratings
				}
				if(this.onlycontent){
					ratings=ratings.filter((rating)=>{
						return rating.text
					})
				}
				return ratings;
			},
			totalScore(){
				let score=0,index=0;
				this.ratings.forEach((rating)=>{
					if(rating.score){
						score+=rating.score;
						index++;
					}
					
				});
				return (score/index).toFixed(1);
			},
			totalTime(){
				let time=0,index=0;;
				this.ratings.forEach((rating)=>{
					if(rating.deliveryTime){
						time+=rating.deliveryTime;
						index++;
					}
				});
				return Math.round(time/index);
			}
			
		},
		methods:{
			changetype(type){
				this.ratingtype=type
			},
			changecontent(){
				this.onlycontent=!this.onlycontent
			}
		}
	}
</script>
<style lang="less" scoped>
	@import "../../common/less/common.less";
	.ratingselect{
		.inf{
			display: flex;
			padding: 18px 0;
			.score-wrap{
				width: 137px;
				text-align: center;
				.border-right(rgb(7, 17, 27));
				.score{
					margin-bottom: 6px;
					line-height: 28px;
					font-size: 14px;
					color:rgb(255, 153, 0);
				}
				.title{
					margin-bottom: 8px;
					line-height: 12px;
					font-size: 12px;
					color: rgb(7, 17, 27);
				}
				.total-rating{
					line-height: 10px;
					font-size: 10px;
					color: rgb(7, 17, 27);
				}
			}
			.rating-item{
				padding:0 24px;
				.severce{
					margin:0 12px 8px 0;
					font-size: 0;
					&:last-child{
						margin-bottom: 0;
					}
					.tag{
						line-height: 18px;
						font-size: 12px;
						color: rgb(7, 17, 27);
					}
					.star-wrap,.time-wrap{
						display: inline-block;
						margin: 0 12px;
					}
					.time-wrap .time{
						line-height: 18px;
						font-size: 12px;
						color: rgb(147, 153, 159);
					}
					.score{
						line-height: 18px;
						font-size: 12px;
						color: rgb(147, 153, 159);
					}
				}
			}
		}
		.ratingselect-wrap{
			margin-top: 18px;
			padding: 0 18px;
		}
		.list{
			padding: 0 18px;
			border-top: 1px solid rgb(7, 17, 27);
			.list-item{
				display: flex;
				padding: 18px 0;
				.border(rgb(7, 17, 27));
				.image-wrap{
					margin-right: 12px;
					img{
						width: 28px;
						height: 28px;
						border-radius: 50%;
					}
				}
				.content{
					flex:1;
					position: relative;
					width: 1%;
					.name{
						line-height: 12px;
						font-size: 10px;
						color: rgb(7, 17, 27);
					}
					.score-wrap{
						margin: 4px p 6px 0;
						.star-wrap{
							display: inline-block;
							margin:4px 6px 6px 0;
							font-size: 0;
						}
						.text{
							line-height: 12px;
							font-size: 10px;
							font-weight: 200;
							color: rgb(147, 153, 159);
						}
					}
					.text{
						margin-bottom: 8px;
						line-height: 18px;
						font-size: 12px;
						color: rgb(7, 17, 27);
						word-break: break-all;
					}
					.food-tag{
						font-size: 0;
						.icon-thumb_up{
							margin-right: 8px;
							line-height: 16px;
							font-size: 12px;
							color: rgb(0, 160, 220);
						}
						.icon-thumb_down{
							margin-right: 8px;
							line-height: 16px;
							font-size: 12px;
							color: rgb(147, 153, 159);
						}
						.tag{
							display: inline-block;
							margin-right: 8px;
							padding: 0 6px;
							line-height: 16px;
							max-width: 6em;
							overflow: hidden;
							white-space: nowrap;
    						text-overflow: ellipsis;
							font-size: 9px;
							color: rgb(147, 153, 159);
							border-radius: 1px;
							border:1px solid rgba(7, 17, 27, 0.1)
						}
					}
				}
			}
		}
	}
</style>