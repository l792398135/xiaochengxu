<template>
	<view class="warp">
		<!-- #ifdef APP-PLUS -->
		<status-bar />
		<!-- #endif -->
		
		<!-- banner -->
		<unicloud-db ref="bannerdb" v-slot:default="{data, loading, error, options}" collection="opendb-banner"
			field="_id,bannerfile,open_url,title" @load="onqueryload">
			<!-- 当无banner数据时显示占位图 -->
			<image v-if="!(loading||data.length)" class="banner-image" src="/static/grid/empty.png" mode="aspectFill" :draggable="false" />
			<uni-swiper-dot v-else class="uni-swiper-dot-box" @clickItem="clickItem" :info="data" 
				:current="current" field="content">
				<swiper class="swiper-box" @change="changeSwiper" :current="swiperDotIndex">
					<swiper-item v-for="(item, index) in data" :key="item._id">
						<view class="swiper-item" @click="clickBannerItem(item)">
							<image class="banner-image" :src="item.bannerfile.url" mode="aspectFill" :draggable="false" />
						</view>
					</swiper-item>
				</swiper>
			</uni-swiper-dot>
		</unicloud-db>
		
		
	<!-- 宫格 -->
		<uni-section :title="$t('grid.grid')" style="margin: 0;" type="line"></uni-section>
		<view class="example-body">
			<uni-grid :column="3" :highlight="true">
				<uni-grid-item>
					<view class="grid-item-box" @click="handleToAccount">
					  <uni-icons type="locked-filled" size="30"></uni-icons>
					  <text class="text">账号管理</text>
					</view>
				</uni-grid-item>
			
				<uni-grid-item>
				  <view class="grid-item-box"  @click="handleToAsset">
				    <uni-icons type="medal" size="30"></uni-icons>
				    <text class="text">资产管理</text>
				  </view>
				</uni-grid-item>
				
				<uni-grid-item>
				  <view class="grid-item-box"  @click="changeGrid">
				    <uni-icons type="medal" size="30"></uni-icons>
				    <text class="text">收入</text>
				  </view>
				</uni-grid-item>
				
				<uni-grid-item>
				  <view class="grid-item-box"  @change="changeGrid">
				    <uni-icons type="medal" size="30"></uni-icons>
				    <text class="text">支出</text>
				  </view>
				</uni-grid-item>
				
			
				
			</uni-grid>
		</view>
	</view>
</template>

<script>
	import {
		mapGetters,
	} from 'vuex';
	import statusBar from "@/uni_modules/uni-nav-bar/components/uni-nav-bar/uni-status-bar";
	export default {
		components: {
			statusBar
		},
		data() {
			return {
				gridList: [],
				current: 0,
				swiperDotIndex: 0
			}
		},
		computed: {
			...mapGetters({
				hasLogin: 'user/hasLogin'
			})
		},
		onLoad() {
			let gridList = []
			for (var i = 0; i < 3; i++) {
				gridList.push( this.$t('grid.visibleToAll') )
			}
			for (var i = 0; i < 3; i++) {
				gridList.push( this.$t('grid.invisibleToTourists') )
			}
			for (var i = 0; i < 3; i++) {
				gridList.push( this.$t('grid.adminVisible') )
			}
			this.gridList = gridList
		},
		methods: {
			handleToAccount() {
			  uni.navigateTo({url:'/pages/my-account-save/list'})
			},
			handleToAsset() {
			  uni.navigateTo({url:'/pages/my-asset-manage/list'})
			},
			change(e) {
				uni.showToast({
					title:this.$t('grid.clickTip') + " " + `${e.detail.index}` + " " + this.$t('grid.clickTipGrid'),
					icon: 'none'
				})
			},
			/**
			 * banner加载后触发的回调
			 */
			onqueryload(data) {
			},
			changeSwiper(e) {
				this.current = e.detail.current
			},
			clickItem(e) {
				this.swiperDotIndex = e
			},
			/**
			 * 点击banner的处理
			 */
			clickBannerItem(item) {
				// 有外部链接-跳转url
				if (item.open_url) {
					uni.navigateTo({
						url: '/pages/common/webview/webview?url=' + item.open_url + '&title=' + item.title,
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}
				// 其余业务处理
			}
		}
	}
</script>

<style>
	/* #ifndef APP-NVUE */
	page {
		display: flex;
		flex-direction: column;
		box-sizing: border-box;
		background-color: #fff;
		min-height: 100%;
		height: auto;
	}
	view {
		font-size: 14px;
		line-height: inherit;
	}
	.example-body {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		flex-wrap: wrap;
		justify-content: center;
		padding: 0;
		font-size: 14px;
		background-color: #ffffff;
	}
	/* #endif */

	/* #ifdef APP-NVUE */
	.warp {
		background-color: #fff;
	}
	/* #endif */

	.example-body {
		flex-direction: column;
		padding: 15px;
		background-color: #ffffff;
	}

	.image {
		width: 50rpx;
		height: 50rpx;
	}

	.text {
		text-align: center;
		font-size: 26rpx;
		margin-top: 10rpx;
	}

	.example-body {
		/* #ifndef APP-NVUE */
		display: block;
		/* #endif */
	}

	.grid-item-box {
		flex: 1;
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding: 15px 0;
	}

	.banner-image {
		width: 750rpx;
		height: 400rpx;
	}

	.swiper-box {
		height: 400rpx;
	}

	.search-icons {
		padding: 16rpx;
	}

	.search-container-bar {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		justify-content: center;
		align-items: center;
		position: fixed;
		left: 0;
		right: 0;
		z-index: 10;
		background-color: #fff;
	}

	/* #ifndef APP-NVUE || VUE3*/
	/deep/
	/* #endif */
	.uni-searchbar__box {
		border-width: 0;
	}

	/* #ifndef APP-NVUE || VUE3 */
	/deep/
	/* #endif */
	.uni-input-placeholder {
		font-size: 28rpx;
	}
</style>
