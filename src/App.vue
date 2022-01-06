<template>
	<div id="app">
		<nav class="nav">
			<img src="./assets/arrow-left.png" class="back" />
			<span class="title">历史心情指数</span>
		</nav>
		<section class="card">
			<div class="user">
				<img src="./assets/logo.png" class="user_icon" />
				<span class="text">Sakura</span>
			</div>
			<div class="data">
				<div class="nums">88</div>
				<div>周平均心情指数</div>
			</div>
		</section>
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
.card {
	margin-top: 15px;
	padding: 50px 0 2px;
	border-top-left-radius: 24px;
	border-top-right-radius: 24px;

	box-shadow: 0px -6px 16px -6px rgba(0, 0, 0, 0.2);
}
.user {
	display: flex;
	justify-content: center;
	align-items: center;
	font-weight: 600;
	.user_icon {
		width: 32px;
		height: 32px;
		border-radius: 50%;
		border: 1px solid #eee;
		margin-right: 10px;
	}
}
.text {
	font-weight: 500;
	font-size: 20px;
}
.nav {
	display: flex;
	justify-content: space-between;
	align-items: center;
	.back {
		width: 21px;
		height: 16px;
	}
	.title {
		flex: 1;
		font-size: 20px;
		font-weight: 600;
		margin-right: 21px;
	}
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
