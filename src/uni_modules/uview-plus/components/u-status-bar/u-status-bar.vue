<template>
	<view
	    :style="[style]"
	    class="u-status-bar"
		:class="[isH5 && 'u-safe-area-inset-top']"
	>
		<slot />
	</view>
</template>

<script>
	import { props } from './props';
	import { mpMixin } from '../../libs/mixin/mpMixin';
	import { mixin } from '../../libs/mixin/mixin';
	import { addUnit, addStyle, deepMerge, getWindowInfo } from '../../libs/function/index';
	/**
	 * StatbusBar 状态栏占位
	 * @description 本组件主要用于状态填充，比如在自定导航栏的时候，它会自动适配一个恰当的状态栏高度。
	 * @tutorial https://uview-plus.jiangruyi.com/components/statusBar.html
	 * @property {String}			bgColor			背景色 (默认 'transparent' )
	 * @property {String | Object}	customStyle		自定义样式 
	 * @example <u-status-bar></u-status-bar>
	 */
	export default {
		name: 'u-status-bar',
		mixins: [mpMixin, mixin, props],
		data() {
			return {
				isH5: false
			}
		},
		created() {
			// #ifdef H5
			this.isH5 = true
			// #endif
		},
		emits: ['update:height'],
		computed: {
			style() {
				const style = {}
				// 状态栏高度，由于某些安卓和微信开发工具无法识别css的顶部状态栏变量，所以使用js获取的方式
				let sheight = getWindowInfo().statusBarHeight
				this.$emit('update:height', sheight)
				if (sheight == 0) {
					this.isH5 = true
				} else {
					style.height = addUnit(sheight, 'px')
				}
				style.backgroundColor = this.bgColor
				return deepMerge(style, addStyle(this.customStyle))
			}
		},
	}
</script>

<style lang="scss" scoped>
	.u-status-bar {
		// nvue会默认100%，如果nvue下，显式写100%的话，会导致宽度不为100%而异常
		/* #ifndef APP-NVUE */
		width: 100%;
		/* #endif */
	}
</style>
