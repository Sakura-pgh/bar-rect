<template>
	<section class="price_gf" v-if="recData">
		<div class="rec_wrapper">
			<div
				v-for="(rec, index) in recData"
				class="rec_box"
				:style="{ width: recW + '%', height: recMaxH + 'px' }"
				:key="rec.id"
			>
				<div v-if="rec.show" class="rec_item_wrapper" @click="changeActive(rec, index)">
					<template v-if="showRecH && rec.height === '0px'">
						<div class="rec_item empty"></div>
						<img src="../assets/none.png" class="status_img" />
					</template>
					<div
						v-else
						class="rec_item"
						:class="rec.active ? 'active ' + rec.type : ''"
						:style="{ height: showRecH ? rec.height : '0', transitionDelay: `${index * 0.1}s`, background: rec.color }"
					>
						<p class="num">{{ rec.value }}</p>
						<img src="../assets/normal.png" class="status_img" v-if="rec.type === 'normal'" />
						<img src="../assets/high.png" class="status_img" v-if="rec.type === 'high'" />
					</div>
				</div>
			</div>
			<div class="center_line"></div>
		</div>
		<div class="v_line"></div>
		<div class="title_box">
			<div
				class="title"
				v-for="(item, index) in recData"
				:style="item.show ? { width: recW * 2 + '%' } : { display: 'none' }"
				:key="item.id"
				@click="changeActive(item, index)"
			>
				<span class="title_text" :class="item.active ? 'active ' + item.type : ''">{{
					item.show ? item.name : ''
				}}</span>
			</div>
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
			default: 250,
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
	computed: {},
	mounted() {
		// 用于呈现高度增长动画效果
		setTimeout(() => {
			this.showRecH = true;
		}, 20);
	},
	methods: {
		changeActive(rec, index) {
			console.log('rec: ', rec.active);
			rec.active = !rec.active;
			this.$set(this.recData, index, rec);
		},
		// 构建所需的长方形数据
		structRecData() {
			const { gfData } = this;
			const recDataLen = gfData.length + 2;
			// 每个长方形的占比宽度
			this.recW = 100 / recDataLen;
			// 找出最大数值，并以此为基点进行分配每一份的高度
			const maxNumber = 100;
			const oneH = this.recMaxH / maxNumber;
			this.recData = gfData.reduce((total, data) => {
				data.height = data.value * oneH + 'px';
				data.show = true;
				if (data.value >= 90) {
					data.color = '#FF823C';
					data.type = 'high';
				} else if (data.value >= 80) {
					data.color = '#52C873';
					data.type = 'normal';
				} else {
					data.color = '#f86a6b';
					data.type = 'other';
				}
				data.active = false;
				return total.concat([], data);
			}, []);
			// .concat({ id: 'last_rec' });
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
		display: flex;
		justify-content: space-around;
		border-top: 1px solid #f2f2f2;
		.center_line {
			height: 1px;
			width: 100%;
			background: #f2f2f2;
			position: absolute;
			top: 50%;
			transform: translateY(-50%);
			z-index: -1;
		}
		.rec_box {
			position: relative;
			display: inline-block;
			vertical-align: middle;
			border-bottom-left-radius: 12px;
			border-bottom-right-radius: 12px;
			// background-color: #f8f8fc;
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
					margin-top: 10px;
					margin-bottom: 4px;
					color: #fff;
					text-align: center;
					font-size: 16px;
					font-weight: 600;
				}
				.status_img {
					position: absolute;
					bottom: 5px;
					left: 50%;
					transform: translateX(-50%);
				}
				.rec_item {
					overflow: hidden;
					text-align: center;
					border-radius: 30px;
					background-color: #f86a6b;
					transition: height 1s ease-out;

					&:nth-child(2n) {
						background-color: pink;
					}
					&.empty {
						height: 84px;
						background: #cfcfcf;
					}
					&.active {
						&.high {
							background: linear-gradient(180deg, #ffa14a 35.42%, #ffcc4a 100%) !important;
							box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
						}
						&.normal {
							background: linear-gradient(180deg, #42f373 42.42%, #a1fd44 100%) !important;
							box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.15);
						}
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
		display: flex;
		justify-content: space-around;
		.title {
			position: relative;
			display: inline-block;
			text-align: center;
			font-size: 14px;
			font-weight: 600;
			color: #030508;
			.title_text {
				padding: 5px;
				&.active {
					background: #fff;
					&.high {
						color: #f36a1b;
						box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.2);
					}
					&.normal {
						color: #52c873;
						box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.15);
					}
				}
			}
		}
	}
}
</style>
