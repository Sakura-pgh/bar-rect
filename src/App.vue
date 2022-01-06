<template>
	<div id="app">
		<nav>
			<span>历史心情指数</span>
		</nav>
		<div class="user">
			<span class="text">Sakura</span>
		</div>
		<div class="data">
			<div class="nums">88</div>
			<div>周平均心情指数</div>
		</div>
		<BarGraph :gfData="gfData" />
	</div>
</template>

<script>
import BarGraph from './components/BarGraph.vue';

export default {
	name: 'app',
	data() {
		return {
			gfData: [
				{ id: '6', name: '六', value: '86' },
				{ id: '7', name: '日', value: '80' },
				{ id: '1', name: '一', value: '0' },
				{ id: '2', name: '二', value: '90' },
				{ id: '3', name: '三', value: '92' },
				{ id: '4', name: '四', value: '97' },
				{ id: '5', name: '五', value: '81' },
			],
		};
	},
	components: {
		BarGraph,
	},
};
</script>

<style lang="scss">
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
}
.user {
	margin-top: 60px;
}
.text {
	font-weight: 500;
	font-size: 20px;
}
.data {
	margin: 10px 0;
	.nums {
		font-weight: 600;
		font-size: 72px;
	}
	p {
		color: #929292;
	}
}
</style>
