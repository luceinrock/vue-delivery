<template>
	<div class="search">
		<HeaderTop title="搜索"></HeaderTop>
		<form class="search_form" @submit.prevent="search">
			<input type="search" name="search" placeholder="请输入商家或美食名称" class="search_input" v-model="key">
			<input type="submit" name="submit" class="search_submit">
		</form>
		<section class="list" v-if="searchShops.length">
			<ul class="list_container">
				<li class="list_li" v-for="(shop, index) in searchShops" :key="index">
					<section class="item_left">
						<img :src="imgBaseUrl + shop.image_path" class="restaurant_img">
					</section>
					<section class="item_right">
						<div class="item_right_text">
							<p>
								<span>{{shop.name}}</span>
							</p>
							<p>月售 {{shop.month_sales||shop.recent_order_num}} 单</p>
							<p>{{shop.delivery_fee||shop.float_minimum_order_amount}} 元起送 / 距离 {{shop.distance}}</p>
						</div>
					</section>
				</li>
			</ul>
		</section>
		<div class="search_none" v-if="emptyResult">很抱歉！无搜索结果</div>
	</div>
</template>

<script>
import HeaderTop from '../../components/HeaderTop/HeaderTop.vue'
import { mapState } from 'vuex'
export default {
	name: 'search',
	data () {
		return {
			key: '',
			emptyResult: false,
			imgBaseUrl: 'http://cangdu.org:8001/img/'
		}
	},
	computed: {
		...mapState(['searchShops'])
	},
	components: {
		HeaderTop
	},
	watch: {
		searchShops (val) {
			if (!val.length) {
				this.emptyResult = true
				console.log(this.emptyResult)
			}
		}
	},
	methods: {
		search () {
			const keyword = this.key
			this.emptyResult = false
			this.$store.dispatch('getSearchShops', keyword)
		}
	}
}
</script>

<style lang="stylus" scoped>
.search  //搜索
	width 100%
	.search_form
		clearFix()
		background #fff
		height 40px
		padding 57px 8px 12px
		input
			height 35px
			padding 0 4px
			border-radius 2px
			font-weight bold
			outline none
			&.search_input
				float left
				width 79%
				border 4px solid #f2f2f2
				font-size 14px
				color #333
				background-color #f2f2f2
			&.search_submit
				float right
				width 18%
				border 4px solid #02a774
				font-size 16px
				color #fff
				background-color #02a774
	.list
		.list_container
			background-color: #fff;
			.list_li
				display: flex;
				justify-content: center;
				padding: 10px
				border-bottom: 1px solid $bc;
			.item_left
				margin-right: 10px
				.restaurant_img
					width 50px
					height 50px
					display block
			.item_right
				font-size 12px
				flex 1
				.item_right_text
					p
						line-height 12px
						margin-bottom 6px
						&:last-child
							margin-bottom 0
	.search_none
		margin: 0 auto
		color: #333
		text-align: center
		margin-top: 0.125rem
</style>
