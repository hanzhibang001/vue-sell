<template>
	<div class="header">
		<div class="content-wrapper">
			<div class="avatar">
				<img width="64" height="64" :src="seller.avatar">
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{seller.name}}</span>
				</div>
				<div class="description">
					{{seller.description}}/{{seller.deliveryTime}}/分钟送达
				</div>
				<div v-if="seller.supports" class="supports">
					<span class="icon" :class="classMap[seller.supports[0].type]"></span>
					<span class="text">{{seller.supports[0].description}}</span>
				</div>
			</div>
			<div v-if="seller.supports" class="support-count" @click="showDetail">
				<span class="count">{{seller.supports.length}}个</span>
				<i class="icon-keyboard_arrow_right"></i>
			</div>
		</div>
		<div class="bulletin-wrapper" @click="showDetail">
			<span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
			<i class="icon-keyboard_arrow_right"></i>
		</div>
		<div class="background">
			<img :src="seller.avatar" width="100%" height="100%">
		</div>
		<transition name="fade">
			<div v-show="detailShow" class="detail">
				<div class="detail-wrapper clearfix">
					<div class="detail-main">
						<h1 class="name">{{seller.name}}</h1>
						<star :size="48" :score="seller.score"></star>
						<div class="title">
							<div class="line"></div>
							<div class="text">优惠信息</div>
							<div class="line"></div>
						</div>
						<ul v-if="seller.supports" class="supports">
							<li class="support-item" v-for="(item,index) in seller.supports">
							<span class="icon" :class="classMap[seller.supports[index].type]"></span>
							<span class="text">{{seller.supports[index].description}}</span>
							</li>
						</ul>
						<div class="title">
							<div class="line"></div>
							<div class="text">商家公告</div>
							<div class="line"></div>
						</div>
						<div class="bulletin">
							<p class="text">{{seller.bulletin}}</p>
						</div>		            
					</div>
				</div>
				<div v-show="detailShow" class="detail-close" @click="hideDetail">
					<i class="icon-close"></i>
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
	import star from '../star/star.vue';

	export default {
		props: {
			seller: {
				type: Object
			}
		},
		data() {
			return {
				detailShow: false
			};
		},
		methods: {
			showDetail() {
				this.detailShow = true;
			},
			hideDetail() {
				this.detailShow = false;
			}
		},
		created() {
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
		},
		components: {
			star
		}
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
 @import "../../common/stylus/mixin.styl";


	.header
		position:relative
		color:#fff
		overflow:hidden
		background:rgba(7,17,27,0.5)
		.content-wrapper
			position:relative
			padding:24px 12px 18px 24px
			font-size:0
			.avatar
				vertical-align:top
				display: inline-block
				img
					border-radius:2px
			.content
				display: inline-block
				margin-left:16px
				.title
					margin:2px 0 8px 0
					.brand
						display:inline-block
						vertical-align:top
						width:30px
						height: 18px
						bg-image('brand')
						background-repeat: no-repeat
						background-size:30px 18px
					.name
						margin-left: 6px
						font-size:16px
						font-weight:bold
						line-height:18px
				.description
					font-size:12px
					line-height:12px
					margin-bottom:10px
				.supports
					.icon
						display:inline-block
						vertical-align:top
						width:12px
						height:12px
						margin-right:4px
						background-size:12px 12px
						background-repeat:no-repeat
						&.decrease
							bg-image('decrease_1')
						&.discount
							bg-image('discount_1')
						&.guarantee
							bg-image('guarantee_1')
						&.invoice
							bg-image('invoice_1')
						&.special
							bg-image('special_1')
					.text
						line-height:12px
						font-size:10px
			.support-count
				position: absolute
				right:12px
				bottom:14px
				padding:0 8px
				height:24px
				line-height:24px
				border-radius:14px
				background:rgba(0, 0, 0, 0.2)
				text-align:center
				.count
					vertical-align: top
					font-size: 10px
				.icon-keyboard_arrow_right
					margin-left:2px
					line-height:24px
					font-size:10px
		.bulletin-wrapper
			position:relative
			height:28px
			line-height:28px
			padding:0 22px 0 12px
			white-space:nowrap
			overflow:hidden
			text-overflow:ellipsis
			background-color:rgba(7,17,27,0.2)
			.bulletin-title
				margin-top:9px
				display:inline-block
				width:22px
				height:12px
				bg-image('bulletin')
				background-size:22px 12px
				background-repeat:no-repeat
			.bulletin-text
				vertical-align: top
				font-size:10px
				font-weight:200
				line-height:28px
				margin:0 4px
			.icon-keyboard_arrow_right	
				position:absolute
				right:12px
				top:10px
				font-size:10px										  
		.background
			position:absolute
			top:0
			left:0
			width:100%
			height:100%
			z-index:-1
			filter:blur(10px)
		.detail
			position:fixed
			z-index:100
			top:0
			left:0
			width:100%
			height:100%
			overflow:auto
			opacity:1
			background:rgba(7,17,27,0.8)
			&.fade-enter-active,&.fade-leave-active
				transition:all 0.5s
			&.fade-enter,&.fade-leave-to
				opacity:0
				background:rgba(7,17,27,0)
			.detail-wrapper
				min-height:100%
				width:100%
				.detail-main
					padding:64px 0 74px 0
					.name
						line-height:16px
						font-size:16px
						font-weight:700
						text-align:center
					.star
						text-align:center
						margin:16px 0 28px 0
						padding:2px 0
					.title
						display: flex
						margin:0 auto 24px auto
						width:80%
						.line
							flex:1
							position: relative
							bottom:6px
							border-bottom:1px solid rgba(255,255,255,0.2)
						.text
							padding:0 12px 0 12px
							font-size:14px
							font-weight:700
					.supports
						width:80%
						margin:0 auto 28px auto
						.support-item
							padding:0 12px
							margin-bottom:12px
							font-size:0
							&:last-child
								margin-bottom:0
							.icon
								display:inline-block
								width:16px
								height:16px
								vertical-align:top
								margin-right:6px
								background-size:16px 16px
								background-repeat:no-repeat
								&.decrease
									bg-image('decrease_2')
								&.discount
									bg-image('discount_2')
								&.guarantee
									bg-image('guarantee_2')
								&.invoice
									bg-image('invoice_2')
								&.special
									bg-image('special_2')
							.text
								line-height:16px
								font-size:12px
					.bulletin
						width:80%
						margin:0 auto
						.text
							margin:0 12px
							font-size:12px
							font-weight:200
							line-height:24px
			.detail-close
				position:relative
				width:32px
				height:32px
				margin:-64px auto 0 auto
				clear:both
				font-size:32px
				
</style>
