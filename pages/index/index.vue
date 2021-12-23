<template>
	<view class="content">
		<cu-custom bgColor="bg-gradual-blue">
			<block slot="content">公寓门禁码</block>
			<block slot="right">
				<button @click="go2Setting" class="cu-btn cuIcon margin-lr-xs" style="background-color: unset;">
					<text class="text-white cuIcon-settingsfill"></text>
				</button>
			</block>
		</cu-custom>

		<view class="qrcode-box">
			<uqrcode ref="uqrcode" />
			<view class="margin-top">{{ tips }}</view>
			<button @click="make(true)" class="cu-btn round bg-green margin-top">
				<text class="cuIcon-refresh margin-right-xs"></text>刷新
			</button>
		</view>

	</view>
</template>

<script>
	import uQRCode from '@/uni_modules/Sansnn-uQRCode/components/uqrcode/common/uqrcode'

	export default {
		data() {
			return {
				qrcodeSize: 256,
				code: '',
				tips: '',
			}
		},
		onLoad() {
			// 在onReady中调用绘制二维码方法，尽量不要在onLoad中
			const code = uni.getStorageSync('code')
			if (!code) {
				uni.redirectTo({ url: './setting' })
				return
			}
			this.code = code
		},
		onReady() {
			this.make()
		},
		onShow () {		
			// 每分钟刷新
			var _this = this
			setInterval(function() {
				let timestamp = Date.parse(new Date())
				if (timestamp % (60 * 1000) == 0 && _this.code) {
					_this.make()
				}
			}, 1000)
		},
		methods: {
			make(isRefresh = false) {
				uni.showLoading({
					mask: true,
					title: '生成中...'
				})

				const code = this.code
				if (!code) {
					uni.redirectTo({ url: './setting' })
					uni.hideLoading()
					return
				}
				let timestamp = Date.parse(new Date()).toString()
				timestamp = timestamp.substr(0, 10)
				timestamp = Number.parseInt(timestamp) + 60
				const codeStr = {
					sfmbm: code,
					sfmyxjzsj: timestamp
				}
				// console.log('codeStr', codeStr)

				this.$refs
					.uqrcode
					.make({
						mode: 'canvas',
						size: this.qrcodeSize,
						text: JSON.stringify(codeStr),
						margin: 10
					})
				const tipsDatetime = this.timestamp2Time(timestamp)
				this.tips = `${tipsDatetime} 前有效`
				uni.hideLoading()
				if (isRefresh) {
					uni.showToast({
						title: '二维码已刷新'
					})
				}
			},
			go2Setting () {
				uni.navigateTo({
					url: './setting'
				})
			},
			timestamp2Time(timestamp) {
				var date = new Date(Number.parseInt(String(timestamp) + '000'))
				var Y = date.getFullYear() + '-'
				var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-'
				var D = (date.getDate() < 10 ? '0' + date.getDate() : date.getDate()) + ' '
				var h = (date.getHours() < 10 ? '0' + date.getHours() : date.getHours()) + ':'
				var m = (date.getMinutes() < 10 ? '0' + date.getMinutes() : date.getMinutes()) + ':'
				var s = (date.getSeconds() < 10 ? '0' + date.getSeconds() : date.getSeconds())
				const strDate = Y + M + D + h + m + s
				return strDate
			}
		}
	}
</script>

<style>
	/* #ifndef APP-NVUE */
	page {
		background-color: #f0f0f0;
	}

	/* #endif */

	.title {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		align-items: center;
		margin-top: 50rpx;
		font-size: 36rpx;
		color: #666666;
	}

	.qrcode-box,
	.image-box,
	.canvas-box {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: column;
		align-items: center;
		margin-top: 50rpx;
	}

	.text {
		/* #ifndef APP-NVUE */
		display: block;
		/* #endif */
		margin-top: 12rpx;
		font-size: 34rpx;
	}

	.image-box {
		width: 400rpx;
		margin-top: 50rpx;
	}

	.image {
		/* #ifndef APP-NVUE */
		display: block;
		/* #endif */
		width: 400rpx;
		height: 400rpx;
	}

	.input {
		width: 600rpx;
		height: 40px;
		margin: 50rpx 0;
		padding: 0 20rpx;
		border: 1px solid #b0b0b0;
		border-radius: 5px;
		background-color: #ffffff;
	}

	.button {
		width: 690rpx;
		margin: 10rpx;
	}

	.button:last-child {
		margin-bottom: 50rpx;
	}

	.component {
		margin-top: 30rpx;
		text-align: center;
	}

	.component-title {
		/* #ifndef APP-NVUE */
		display: inline-block;
		/* #endif */
		padding: 20rpx 40rpx;
		border-bottom: 2px solid #d8d8d8;
		font-size: 36rpx;
	}

	.component-buttons {
		margin-top: 30rpx;
	}
</style>
