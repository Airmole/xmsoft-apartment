<template>
	<view class="content">
		<cu-custom bgColor="bg-gradual-blue" isGoHome="true">
			<block slot="content">设置</block>
		</cu-custom>

		<form>
			<view class="text-center">
				<image style="height: 300px;" src="../../static/logo.png" mode="heightFix"></image>
			</view>
			<view class="cu-form-group radius margin-top margin-lr">
				<textarea maxlength="-1" v-model="code" placeholder='请输入或扫描识别"e鹭安 - 个人平安码"'></textarea>
			</view>
		</form>
		
		<view class="padding flex flex-wrap justify-between margin-lr-xl align-center">
			<button @click="scan()" class="cu-btn round bg-green"><text class="cuIcon-scan margin-right-xs"></text>扫描识别</button>
			<button @click="save()" class="cu-btn round bg-green"><text class="cuIcon-check margin-right-xs"></text>确认保存</button>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				code: ''
			}
		},
		onLoad() {
			const code = uni.getStorageSync('code')
			if (code) {
				this.code = code
			}
		},
		onReady() { },
		methods: {
			scan () {
				var _this = this
				uni.scanCode({
					scanType: ['qrCode'],
					success: function (res) {
						const json = JSON.parse(res.result)
						if (!json.sfmbm) {
							uni.showToast({
								title: '未识别到e鹭安个人平安码'
							})
							return
						}
						_this.code = json.sfmbm
					}
				})
			},
			save () {
				if (!this.code) {
					uni.showToast({
						icon: 'none',
						title: 'e鹭安平安码不得为空'
					})
					return
				}
				
				uni.setStorageSync('code', this.code)
				uni.showToast({
					title: '已保存'
				})
				setTimeout(function(){
					uni.redirectTo({
						url: './index'
					})
				}, 1500)
			}
		}
	}
</script>

<style>

</style>
