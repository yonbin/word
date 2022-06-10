<template>
	<view class="container">
		<div id="start" style="padding: 10px;" v-show="showStart">
			<button type="primary" @click="start">开 始</button>
		</div>
		<div id="content" v-show="showContent">
			<div class="name">{{currentWord.name}}
				<uni-icons class="name-icon" type="sound-filled" size="30" @click="play"></uni-icons>
			</div>
			<div class="phone">[{{currentWord.usphone}}]</div>

			<div class="trans" v-for="item in currentWord.trans" :key="item">
				{{item}}
			</div>
			<div class="op-btn">
				<uni-row :gutter="20">
					<uni-col :span="12">
						<button type="primary" @click="prev">上一个</button>
					</uni-col>
					<uni-col :span="12">
						<button type="primary" @click="next">下一个</button>
					</uni-col>
				</uni-row>
			</div>
		</div>
	</view>
</template>

<script>
	import nce_2 from "./nce_2.json"
	export default {
		data() {
			return {
				index: 0,
				currentWord: {},
				controls: false,
				showStart: true,
				showContent: false,
				soundUrl: 'http://dict.youdao.com/dictvoice?type=0&audio='
			}
		},
		methods: {
			start() {
				this.showStart = false;
				this.showContent = true;
				this.play();
			},
			next() {
				if (this.index >= nce_2.length) {
					return;
				}
				this.index++;
				this.currentWord = nce_2[this.index];
				this.play();
			},
			prev() {
				if (this.index === 0) {
					return;
				}
				this.index--;
				this.currentWord = nce_2[this.index];
				this.play();
			},
			play() {
				this.currentWord = nce_2[this.index];
			
				if (typeof Audio != "undefined") {
					var audio1 = new Audio(this.soundUrl + this.currentWord.name);
					audio1.play();
				} else {
					let audio2 = wx.createInnerAudioContext();
					audio2.src=this.soundUrl + this.currentWord.name;
					audio2.play();
				}
			}
		}
	}
</script>

<style>
	.container {
		position: relative;
		top: 23%;
		font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
		-webkit-font-smoothing: antialiased;
		color: rgba(75, 85, 99, 1);
	}

	.name {
		height: 45px;
		display: flex;
		justify-content: center;
		font-size: 38px;
	}

	.name-icon {
		margin-left: 5px;
	}

	.phone {
		height: 36px;
		display: flex;
		justify-content: center;
		font-size: 22px;
	}

	.trans {
		display: flex;
		justify-content: center;
		list-style: none;
		font-size: 20px;
		line-height: 28px;
	}

	.op-btn {
		padding: 25px;
	}
</style>
