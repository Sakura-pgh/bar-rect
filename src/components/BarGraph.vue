<template>
	<section class="price_gf" v-if="recData">
		<div class="rec_wrapper">
			<div
				v-for="(rec, index) in recData"
				class="rec_box"
				:style="{ width: recW + '%', height: recMaxH + 'px' }"
				:key="rec.id"
			>
				<div v-if="rec.show" class="rec_item_wrapper">
					<template v-if="showRecH && rec.height === '0px'">
						<div class="rec_item empty"></div>
					</template>
					<div
						v-else
						class="rec_item"
						:style="{ height: showRecH ? rec.height : '0', transitionDelay: `${index * 0.1}s`, background: rec.color }"
					>
						<p class="num">{{ rec.value }}</p>
					</div>
				</div>
			</div>
		</div>
		<div class="v_line"></div>
		<div class="title_box">
			<span
				class="title"
				v-for="item in recData"
				:style="item.show ? { width: recW * 2 + '%' } : { display: 'none' }"
				:key="item.id"
				>{{ item.show ? item.name : '' }}</span
			>
		</div>
	</section>
</template>

<script>
export default {
	name: 'BarGraph',
	props: {
		// 柱状图的数据
		gfData: {
			required: true,
			type: Array,
			default: null,
		},
		// 长方形最大高度
		recMaxH: {
			required: false,
			type: Number,
			default: 220,
		},
	},
	data() {
		return {
			// 每一个长方形的宽度%
			recW: 0,
			// 所需的长方数组
			recData: null,
			// 展示圆柱，形成动画效果
			showRecH: false,
		};
	},
	created() {
		this.structRecData();
	},
	mounted() {
		// 用于呈现高度增长动画效果
		setTimeout(() => {
			this.showRecH = true;
		}, 20);
	},
	methods: {
		// 构建所需的长方形数据，所需的长方形数目是 gfData数量的2倍+1
		structRecData() {
			const { gfData } = this;
			const recDataLen = gfData.length * 2 + 1;
			// 每个长方形的占比宽度
			this.recW = 100 / recDataLen;
			// 找出最大数值，并以此为基点进行分配每一份的高度
			const maxNumber = 100;
			const oneH = this.recMaxH / maxNumber;
			this.recData = gfData
				.reduce((total, data) => {
					data.height = data.value * oneH + 'px';
					// 只显示真正需要展示的长方形
					data.show = true;
					if (data.value >= 90) {
						data.color = '#FF823C';
					} else if (data.value >= 80) {
						data.color = '#52C873';
					} else {
						data.color = '#f86a6b';
					}
					const temp = JSON.parse(JSON.stringify(data));
					temp.id = temp.id + '_temp';
					temp.height = this.recMaxH;
					temp.show = false;
					return total.concat(temp, data);
				}, [])
				.concat({ id: 'last_rec' });
		},
	},
};
</script>

<style scoped lang="scss">
.price_gf {
	padding-bottom: 0;
	.rec_wrapper {
		position: relative;
		white-space: nowrap;
		.rec_box {
			position: relative;
			display: inline-block;
			vertical-align: middle;
			border-bottom-left-radius: 12px;
			border-bottom-right-radius: 12px;
			background-color: #f8f8fc;
			z-index: 8;
			&:nth-child(2n) {
				z-index: 7;
				border-bottom-left-radius: 16px;
				border-bottom-right-radius: 16px;
			}
			$sinkH: -6px;
			&:nth-child(2) {
				// bottom: -6px;
				border-bottom-left-radius: 0;
			}
			&:nth-last-child(3) {
				// bottom: -6px;
				border-bottom-right-radius: 0;
			}
			.rec_item_wrapper {
				position: absolute;
				bottom: 0;
				width: 100%;
				.num {
					margin-top: 0;
					margin-bottom: 4px;
					color: #fff;
					text-align: center;
					font-size: 14px;
				}
				.rec_item {
					overflow: hidden;
					text-align: center;
					border-radius: 12px;
					background-color: #f86a6b;
					background: linear-gradient(to top, #f86a6b, #fd558f);
					transition: height 1s ease-out;
					&:nth-child(2n) {
						background-color: pink;
					}
					&.empty {
						height: 44px;
						background: #cfcfcf;
					}
				}
			}
		}
		$bottomRecBom: 20px;
		$bottomRecH: 40px;
		$bottomRecRadius: 20px;
		.rec_box_first,
		.rec_box_last {
			position: absolute;
			height: 14px;
			bottom: $bottomRecH - $bottomRecBom;
			border-radius: 14px;
			background-color: #f86a6b;
			z-index: 9;
		}
		.rec_box_first {
			border-top-left-radius: $bottomRecRadius;
			border-bottom-left-radius: $bottomRecRadius;
		}
		.rec_box_last {
			border-top-right-radius: $bottomRecRadius;
			border-bottom-right-radius: $bottomRecRadius;
		}
		.bottom_rec {
			position: relative;
			bottom: $bottomRecBom;
			height: $bottomRecH;
			background-color: #f86a6b;
		}
	}
	$vLineMb: 14px;
	.v_line {
		height: 6px;
		margin-bottom: $vLineMb;
		border-radius: 4px;
		background-color: #fff;
	}
	.title_box {
		font-size: 0;
		box-sizing: border-box;
		padding-bottom: $vLineMb;
		.title {
			position: relative;
			display: inline-block;
			text-align: center;
			font-size: 14px;
			color: #030508;
		}
	}
}
</style>
