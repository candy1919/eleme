<template>
	<div class="star" :class="starType">
		<span v-for="itemclass in itemclasses" :class="itemclass" class="star-item"></span>
	</div>
</template>
<script>
	const LEN=5;
	const STARTON='on';
	const STARTHALF='half';
	const STARTOFF='off';
	export default{
		props:{
			'size':Number,
			'score':[Number,String]
		},
		computed:{
			starType(){
				return 'star-'+this.size;
			},
			itemclasses(){
				let result=[];
				let score=Math.floor(this.score*2)/2;
				let hasDecimal=Boolean(score%1!==0);
				let integer=Math.floor(score);
				for(let i=0;i<integer;i++){
					result.push(STARTON);
				}
				if(hasDecimal){
					result.push(STARTHALF);
				}
				while(result.length<LEN){
					result.push(STARTOFF)
				}
				return result;
			}
		}
	}
</script>
<style lang="less">
	.star{
		.star-item{
			display: inline-block;
		}
		&.star-48{
			.star-item{
				width: 20px;
				height: 20px;
				margin-right: 11px;
				background-size: 100% 100%;
				&:last-child{
					margin-right: 0;
				}
				&.on{
				background-image: url('star48_on@2x.png');
				}
				&.half{
					background-image: url('star48_half@2x.png');
				}
				&.off{
					background-image: url('star48_off@2x.png');
				}
			}
		}
		&.star-36{
			.star-item{
				width: 15px;
				height: 15px;
				margin-right: 5px;
				background-size: 100% 100%;
				&:last-child{
					margin-right: 0;
				}
				&.on{
				background-image: url('star36_on@2x.png');
				}
				&.half{
					background-image: url('star36_half@2x.png');
				}
				&.off{
					background-image: url('star36_off@2x.png');
				}
			}
		}
		&.star-24{
			.star-item{
				width: 10px;
				height: 10px;
				margin-right: 3px;
				background-size: 100% 100%;
				&:last-child{
					margin-right: 0;
				}
				&.on{
				background-image: url('star24_on@2x.png');
				}
				&.half{
					background-image: url('star24_half@2x.png');
				}
				&.off{
					background-image: url('star24_off@2x.png');
				}
			}
		}
	}
</style>